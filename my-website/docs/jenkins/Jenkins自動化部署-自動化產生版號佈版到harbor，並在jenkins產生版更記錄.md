# 1.  Jenkins自動化部署-自動化產生版號佈版到harbor，並在jenkins產生版更記錄

還沒有安裝Jenkins? 請參照這篇[[Jenkins自動化部署-安裝教學]]

## 1.1.  簡介
自動化產生版號-將gitlab&jenkins&harbor進行串聯

## 1.2. 專案結構

```
20240303
├─ CHANGELOG.md
├─ check_commit_message.sh
├─ custom-release-notes-generator.js
├─ custom.html
├─ deploy_harbor.sh
├─ docker-compose.yml
├─ dockerfiles
│  ├─ apache
│  │  ├─ apache2
│  │  │  ├─ apache2.conf
│  │  │  ├─ conf-available
│  │  │  │  ├─ charset.conf
│  │  │  │  ├─ localized-error-pages.conf
│  │  │  │  ├─ other-vhosts-access-log.conf
│  │  │  │  ├─ security.conf
│  │  │  │  └─ serve-cgi-bin.conf
│  │  │  ├─ conf-enabled
│  │  │  │  ├─ charset.conf
│  │  │  │  ├─ localized-error-pages.conf
│  │  │  │  ├─ other-vhosts-access-log.conf
│  │  │  │  ├─ security.conf
│  │  │  │  └─ serve-cgi-bin.conf
│  │  │  ├─ envvars
│  │  │  ├─ magic
│  │  │  ├─ mods-available
│  │  │  ├─ mods-enabled
│  │  │  ├─ ports.conf
│  │  │  ├─ sites-available
│  │  │  │  ├─ 000-default.conf
│  │  │  │  └─ default-ssl.conf
│  │  │  └─ sites-enabled
│  │  │     └─ 000-default.conf
│  │  ├─ Dockerfile_vscodeapache
│  │  └─ letsencrypt
│  │     └─ live
│  │        └─ markweb.idv.tw
│  │           ├─ cert.pem
│  │           ├─ chain.pem
│  │           ├─ fullchain.pem
│  │           ├─ privkey.pem
│  │           └─ README
│  └─ vscode
│     └─ Dockerfile_vscode
├─ Jenkinsfile
├─ package-lock.json
├─ package.json
├─ README.md
├─ requirements.txt
├─ send_line_notify.sh
└─ style.css

```

## 1.3. 目錄

- [1.  Jenkins自動化部署-自動化產生版號佈版到harbor，並在jenkins產生版更記錄](#1--jenkins自動化部署-自動化產生版號佈版到harbor並在jenkins產生版更記錄)
  - [1.1.  簡介](#11--簡介)
  - [1.2. 專案結構](#12-專案結構)
  - [1.3. 目錄](#13-目錄)
    - [1.3.1.  執行步驟](#131--執行步驟)
    - [1.3.2. 最終結果](#132-最終結果)


<!--more-->
### 1.3.1.  執行步驟

在`check_commit_message.sh`撰寫以下程式碼

<div class="code-filename">
<i class="fa fa-file-code-o"></i>
</div>
```bash
#!/bin/bash
#+-------------------------------------腳本說明--------------------------------------------+
# 提交訊息檢查腳本
# 使用方式: ./check_commit_message.sh
#
# (C) 2023 - markhsu - licensed under markweb License v1.
# 
#+----------------------------------------------------------------------------------------+
# 取得提交訊息
COMMIT_MESSAGE=$1
# 定義HEADER正規表達式
PATTERN="^(feat|fix|docs|style|refactor|test|chore|feat!|fix!): .*"
# 定義FOOTER正規表達式
FOOTER_PATTERN="^redmine: #[0-9]+$"
echo "我的提交訊息是:$COMMIT_MESSAGE"
# 检查HEADER提交訊息是否符合格式
if [[ ! "$COMMIT_MESSAGE" =~ $PATTERN ]]; then
    echo "
❌ 請檢查您的提交訊息是否正確喔~
🎸feat: 新增/修改功能 (Feature)
🐛fix: 修正 Bug (bug fix)
⚡️ perf: 提高效能的程式碼修正
💡refactor: 重構 or 優化，不屬於 bug 也不屬於新增功能等
🏹release: 新增正式釋出的 release commit 訊息
💄style: 修改程式碼格式或風格，不影響原有運作
💍test: 增加測試功能           
    "
    exit 1
fi
# 消息正確就會到這邊
echo "✅ 恭喜，您的提交訊息是正確的."
exit 0
```

在`deploy_harbor.sh`撰寫以下程式碼

```bash
#!/bin/bash
#+-------------------------------------腳本說明--------------------------------------------+
# 自動化部署到HARBOR，並自動生成版號
# 使用方式: ./deploy_harbor.sh
#
# (C) 2023 - markhsu - licensed under markweb License v1.
# 
#+----------------------------------------------------------------------------------------+

#+----------------------------------------------------------------------------------------+
#                                     接收參數
#+----------------------------------------------------------------------------------------+
# 提取参数
HARBOR_REGISTRY="$1"
HARBOR_USERNAME="$2"
HARBOR_PASSWORD="$3"
INITIAL_VERSION="$4"
image_name="$5"
dockerfile="$6"
HARBOR_PROJECT="$7"
#+----------------------------------------------------------------------------------------+
#                                     抓取版號
#+----------------------------------------------------------------------------------------+
#如果版號為空則給初始版號，否則就抓取git版號
version=$(git describe --tags --abbrev=0 | sed 's/^v//')
echo "Git版號是：$version"
if [ -z "$version" ]; then
    version="${INITIAL_VERSION}"
    echo "初始版號是：$version"
fi

#+----------------------------------------------------------------------------------------+
#                                     抓取commit message
#+----------------------------------------------------------------------------------------+
#抓取commit message來判斷要跳大版號、中版號、小版號
commit_message=$(git log --format=%B -n 2 | tail -n 1)
echo "提交訊息為：$commit_message"
#重大變更
if echo "$commit_message" | grep -q "BREAKING CHANGE"; then
    version_parts=$(echo $version | cut -d"." -f 1)
    echo "提交的重大更新版號是：$version_parts"
    major=$(($version_parts+1))
    echo "大版號是：$major"
    version="${major}.0.0"
    echo "串接版號是：$version"
#feat
elif echo "$commit_message" | grep -q "feat"; then
    version_parts=$(echo $version | cut -d"." -f 2)
    echo "取出的feat版號是：$version_parts"
    minor=$(($version_parts+1))
    echo "中版號是：$minor"
    version="${version_parts}.${minor}.0"
    echo "串接版號是：$version"
#fix
elif echo "$commit_message" | grep -q "fix"; then
    version_parts_major=$(echo $version | cut -d"." -f 1)
    version_parts_minor=$(echo $version | cut -d"." -f 2)
    version_parts_patch=$(echo $version | cut -d"." -f 3)
    echo "提交的fix大版號是：$version_parts_major"
    echo "提交的fix中版號是：$version_parts_minor"
    echo "提交的fix小版號是：$version_parts_patch"
    patch=$(($version_parts_patch+1))
    echo "小版號是：$patch"
    version="${version_parts_major}.${version_parts_minor}.${patch}"
    echo "串接版號是：$version"
#其他類型
else
    version_parts_major=$(echo $version | cut -d"." -f 1)
    version_parts_minor=$(echo $version | cut -d"." -f 2)
    version_parts_patch=$(echo $version | cut -d"." -f 3)
    echo "提交的fix大版號是：$version_parts_major"
    echo "提交的fix中版號是：$version_parts_minor"
    echo "提交的fix小版號是：$version_parts_patch"
    patch=$(($version_parts_patch+1))
    echo "小版號是：$patch"
    version="${version_parts_major}.${version_parts_minor}.${patch}"
    echo "串接版號是：$version"
fi

echo "提取的檔案名稱：$image_name"
echo "Dockerfile檔案名稱：$dockerfile"

#+----------------------------------------------------------------------------------------+
#                                     抓取git commit sha
#+----------------------------------------------------------------------------------------+
commit_sha=$(git rev-parse --short HEAD)
echo "git commit sha：$commit_sha"
version="v${version}-${commit_sha}"
echo "最終串接版號為：$version"

#+----------------------------------------------------------------------------------------+
#                                     部署到harbor
#+----------------------------------------------------------------------------------------+
echo "DEBUG: docker build -t ${HARBOR_REGISTRY}/${HARBOR_PROJECT}/my${image_name}:$version --label "commit_message=${commit_message}" --build-arg COMMIT_SHA=${commit_sha} -f $dockerfile"
docker build -t ${HARBOR_REGISTRY}/${HARBOR_PROJECT}/my${image_name}:$version --label "commit_message=${commit_message}" --build-arg COMMIT_SHA=${commit_sha} -f $dockerfile .
docker login -u ${HARBOR_USERNAME} -p ${HARBOR_PASSWORD} ${HARBOR_REGISTRY}
docker push ${HARBOR_REGISTRY}/${HARBOR_PROJECT}/my${image_name}:$version
```


在`Jenkinsfile`撰寫以下程式碼


```groovy
pipeline {
    agent any

    parameters {
        string(name: 'HARBOR_REGISTRY', defaultValue: 'markweb.idv.tw:29443', description: 'Harbor倉庫地址')
        string(name: 'HARBOR_USERNAME', defaultValue: 'admin', description: 'Harbor用户名')
        password(name: 'HARBOR_PASSWORD', defaultValue: 'admin', description: 'Harbor密碼')
        string(name: 'INITIAL_VERSION', defaultValue: '1.0.0', description: '初始版本號')
        string(name: 'EMAIL_RECIPIENTS', defaultValue: 'markhsu0704@gmail.com', description: '收件人地址')
        string(name: 'LINE_NOTIFY_TOKEN', defaultValue: 'MMvIR1dQ5g1uPYR8FEMN4d3gesRC1WSsBnNKApn7LKW', description: 'line notify 的token')
        string(name: 'DOCKER_PROJECT', defaultValue: 'myvscoderepo', description: 'Harbor倉庫名稱')
    }

    stages {
        stage('從gitlab拉取程式碼') {
            steps {
                script {
                    // 抓取GITLAB程式碼
                    git branch: 'master', credentialsId: 'gitlab', url: 'ssh://git@markweb.idv.tw:2222/dockercomposeteam/codeserver-dockercompose.git'
                }
            }
        }

        stage('部署腳本') {
            steps {
                script {
                    // 部署腳本
					def dockerfiles = sh(script: 'find dockerfiles -mindepth 2 -maxdepth 2 -type f -name "Dockerfile_*"', returnStdout: true).trim().split('\n')
                    echo "dockerfiles: ${dockerfiles}"
                    for (def dockerfile in dockerfiles) {
                        def imageName = dockerfile.tokenize('_')[-1].tokenize('/')[0]
                        echo "Image Name: ${imageName}"
                        echo "Dockerfile: ${dockerfile}"
                        sh "sh deploy_harbor.sh ${HARBOR_REGISTRY} ${HARBOR_USERNAME} ${HARBOR_PASSWORD} ${INITIAL_VERSION} ${imageName} ${dockerfile} ${DOCKER_PROJECT}"
                    }

                }
            }
        }
    }

    post {
        always {
            // 安装 curl & pandoc
            script {
                sh '''
                if ! command -v curl &> /dev/null; then sudo apt update && sudo apt install -y curl; fi
                if ! command -v pandoc &> /dev/null; then apt update && apt install -y pandoc; fi
                '''
            }

            script {
                def buildResult = currentBuild.currentResult
                // 執行LINE NOTIFY腳本
                sh """
                echo "部署结果: ${buildResult}"
                sh send_line_notify.sh $JOB_NAME $BUILD_NUMBER $BUILD_URL $GIT_BRANCH $GIT_COMMIT $WORKSPACE $LINE_NOTIFY_TOKEN ${buildResult}
                """
                // 讀取HTML模板內容
                def customHtmlTemplate = readFile('custom.html')
                // 寄送郵件
                emailext (
                    subject: "$JOB_NAME-#$BUILD_NUMBER-${buildResult}",
                    body: customHtmlTemplate,
                    mimeType: 'text/html',
                    to: "markhsu0704@gmail.com"
                )
                
                // 移除 markdown 文件中的标题标记 
                // 使用 pandoc 将 markdown 转换为 HTML
                // 将 Bootstrap 5 的 CSS 文件下载到 Jenkins 作业目录
                // 在 HTML 文件中引用 Bootstrap 5 的 CSS 文件
                sh '''
                sed -i 's/### //' CHANGELOG.md 
                sed -i 's/## //' CHANGELOG.md              
                pandoc -f markdown -t html -o changelog.html CHANGELOG.md
                '''

                // 读取自定义 CSS 样式
                def cssContent = readFile('style.css')
                echo "Changelog CSS content:\n>${cssContent}"

                 // 讀取生成的 HTML 文件
                def changelogHtml = readFile('changelog.html')


                // 在 HTML 文件的开头插入居中的 H1 标题
                changelogHtml = "<style>${cssContent}</style><h1>$JOB_NAME-#$BUILD_NUMBER-Git版本變更紀錄</h1>\n" + changelogHtml

                changelogHtml = "<link rel=\"stylesheet\" href=\"https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css\">" + changelogHtml
                              
                 // 印出 changelogHtml 內容以進行檢查
                echo "Changelog HTML content:\n${changelogHtml}"
                
                // 将变更日志保存到文件
                writeFile file: 'changelog.html', text: changelogHtml


                // 归档 HTML 文件
                archiveArtifacts artifacts: 'changelog.html', onlyIfSuccessful: false
                
                // 在 Jenkins 构建页面上添加 HTML 文件的链接
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: false,
                    keepAll: true,
                    reportDir: '.',
                    reportFiles: 'changelog.html',
                    reportName: 'Gitlab版本變更紀錄'
                ])
            }
        }
    }
}


```

在`style.css`撰寫以下程式碼

```css
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@100..900&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@100..900&family=Nunito:ital,wght@0,200..1000;1,200..1000&display=swap');
/* 设置页面全局字体 */
body {
    font-family: "Nunito","Noto Sans TC", sans-serif;
    margin:10px 12px;
    background-color: #f1f1f1;
}

/* 设置标题样式 */
h1 {
    color: #1a5182;
    font-size: 24px;
    border: 2px dashed #1a5182;
    border-radius: 10px;
    text-align: center;
    padding: 1rem;
    margin: 1rem;
    border-left: 5px solid
}

p{
    font-size:16px;
    color:#1a5182 !important;
}

/* 设置列表样式 */
ul {
    list-style-type: none;
    padding: 0;
}

li {
    margin-bottom: 10px;
    margin-left: 25px;
    font-size:14px;
    color:#518fc7;
}

/* 设置链接样式 */
a {
    color: #1a5182;
    text-decoration: none;
}

a:hover {
    color:#518fc7;
    text-decoration: underline;
    transition: all 0.3s;
}

```

### 1.3.2. 最終結果

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/jaP22949f2e-upload-86371b3da1675b13408b5d3327445598.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/jaP22949f2e-upload-86371b3da1675b13408b5d3327445598.png)


[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/zw9bff9a957-upload-3a5ce7dd0c7a060251558d777a5aed1a.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/zw9bff9a957-upload-3a5ce7dd0c7a060251558d777a5aed1a.png)


[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/0IG40ba0c52-upload-801f003b3abe4b874a8ca6236f686267.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/0IG40ba0c52-upload-801f003b3abe4b874a8ca6236f686267.png)


[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/BWs6405919e-upload-94eaab7eab23b2c586bb72f3126c2896.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/BWs6405919e-upload-94eaab7eab23b2c586bb72f3126c2896.png)