# Pipeline

## Pipelineとは

JenkinsにContinuous Delivery Pipelineを実装するためのプラグイン

* CD Pipeline
  * バージョン管理からユーザや顧客に提供するためのプロセスの自動化の表現
  * 複数のステージのテスト、デプロイを通じてソフトウェアをビルド
  * Pipelineはツールセットを提供
  * DSLでPipelin codeをモデリング

Pipelineの定義はJenkinsfileで記述。 	
Pipeline-as-codeの基礎である。

### Pipeline syntax

Pipeline syntaxは２種類ある。

* Declarative 
* Scripted 

Declarative PiplelineはKenkins Pipelineの最近の機能

* Scripted Pipeline以上の機能を提供
* Pipeline code を読み書きしやすい。

Component( ステップなど) の多くはJenkinsファイルで記述する。	
Declarative , Scripted pipelineの両方が共通。

### なぜPipelineか

Pipelineの追加はJenkinsにパワフルな自動化ツールセットを追加する。	
Pipeline の多くの機能に利点がある
* Code
  * pipelineはコードで実装
* Durable(耐久性)
  * pipelineはjenkins masterの計画あり、なし両方の再起動でも助かる。
* Pausable(一時停止可能な)
  * pipelineは任意に停止、入力待機など可能
* Versatile( 多用途)
  * 複雑な現実のCDの要求をサポート
  * ex. fork / join / loop など
* Extensible(拡張性)
  * DSLの拡張

Jenkinsはフリースタイルジョブのチェーンでシーケンシャルタスクを実現できる。
### Pipeline concept

| name      | desc| 
|-----------|-----|
| Pipeline	|  CD Pipelineのユーザ定義モデル。ビルドプロセスの定義|
| Node		| Jenkins環境の一部、Pipelineを実行できるマシン	|
| Stage		| pipeline全体を通して実行するタスクのサブセットの定義。Jenkins Pipeline status/progressの可視化に使える|
| Step		| 単一のタスク。stepはjenkinsに何をするかを伝える。|


## Tips

### 一部のStageをSlaveで実行したい

### ユースケース

masterはLinux上で動かす。
Windows上でも動かす必要があるジョブを構築する必要がある。

* slaveジョブを登録
* Pipelineのbuild jobにslaveのジョブを登録する。

```
node {
 stage('first-stage'){
    build job: 'slave-job'
 }
}
```


## Jenkins Pipelineの使い方

* 新規ジョブ作成
* 「パイプライン」で作成
* 「設定」でパイプラインで「Scriped Pipeline」を選択
* 「ビルド実行」を実行
* ```tool M3```の設定がないとエラーが出る。
* Jenkinsの管理 -> Global Tool ConfigurationのMavenの名前を「M3」に設定する。
* 「ビルド実行」を実行する。
* ビルドが成功する。

## Pipelineで別Jobを実行

* 実行したいジョブを作成
  * FreeStyleJobを作成して、sleep 10だけするジョブを作成
* stage:Resultの後ろに以下のstageを追加
```
    stage('downstream'){
        build job:'FreestyleJob1'
    } 
```
* FreeStyleJobのsleep 300にしてみる。
* stageが5minで完了するようになった。

## Jobをcurlで実行する
### 目的

stageからフリースタイルジョブを実行した時に別マシンのジョブを実行したい。
### 実装方法

まずはcurlで呼び出す。

* API Keyを取得する。
  * http://localhost:8080/user/${user}/configure
* curlでビルド実行  
  ```curl -v -X POST -u ${user}:${apiToken} http://localhost:8080/job/{$projectName}/build```
* curlでプロジェクトがビルド完了かどうかチェックを行う。  
``` curl -v -X POST -u ${USER}:${API_TOKEN} ${JENKINS_URL}/job/${PROJECT_NAME}/lastBuild/api/json
```

※Web API一発で完了まで待ってくれるAPIがないため、自作する必要がある。

非常に管理しづらい。

個人的には

* 別マシンのジョブ用のstageを追加し、build jobを呼び出す。
* build jobで呼び出すJobはslave登録して、別マシンで動かす




## reference

* [本家サイト](https://www.jenkins.io/doc/book/pipeline/)
* [ジョブの完了待ちについて記載しているブログ](https://kamatama41.hatenablog.com/entry/2018/03/30/113135)


以上
