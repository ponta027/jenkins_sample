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


  

## reference

[本家サイト](https://www.jenkins.io/doc/book/pipeline/)

https://www.kimullaa.com/entry/2017/01/31/002055


以上
