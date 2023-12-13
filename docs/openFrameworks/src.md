---
title: srcフォルダ メモ
layout: default
parent: openFrameworks
---

# srcフォルダ

projectGeneratorによって作成されたプロジェクトに存在するsrcフォルダの内容に関してのメモです。

今後自分がoFで開発したくなったとき用です。

フォルダのパスは openFrameworksのルート/apps/myApps/プロジェクト名/src です。

srcフォルダには以下のファイルが存在します。

- main.cpp
- ofApp.cpp
- ofApp.h

## main.cpp

## ofApp.cpp

### void ofApp::setup()

- プログラム開始時に1回呼び出されます。
- 変数の初期化、初期条件の設定、リソースの読み込みなどに使用されます。

### void ofApp::update()

- プログラムのメインループで連続的に呼び出されます。
- アプリケーションの状態を更新するために使用されます。

### void ofApp::draw()

- プログラムのメインループで`update()`関数の後に連続的に呼び出されます。
- 画面に描画するために使用されます。

### void ofApp::exit()

- アプリケーションが終了する直前に呼び出されます。
- リソースをクリーンアップしたり、プログラムが閉じる前に必要なアクションを実行するために使用できます。

### void ofApp::keyPressed(int key)

- キーボードのキーが押されたときに呼び出されます。
- ユーザーの入力を処理するために使用されます。

### void ofApp::keyReleased(int key)
- キーボードのキーが離されたときに呼び出されます。

### void ofApp::mouseMoved(int x, int y)
- マウスが移動したときに呼び出されます。

### void ofApp::mouseDragged(int x, int y, int button)

- マウスボタンを押したままマウスが移動したときに呼び出されます。

### void ofApp::mousePressed(int x, y int, int button)
- マウスボタンが押されたときに呼び出されます。

### void ofApp::mouseReleased(int x, int y, int button)

- マウスボタンが離されたときに呼び出されます。

### void ofApp::mouseScrolled(int x, int y, float scrollX, float scrollY)

- マウスホイールがスクロールされたときに呼び出されます。

### void ofApp::mouseEntered(int x, int y)

- マウスがウィンドウに入ったときに呼び出されます。

### void ofApp::mouseExited(int x, int y)

- マウスがウィンドウから出たときに呼び出されます。

### void ofApp::windowResized(int w, int h)
- アプリケーションウィンドウのサイズが変更されたときに呼び出されます。

### void ofApp::gotMessage(ofMessage msg)

- アプリケーションがメッセージを受信したときに呼び出される関数（コードには具体的な処理が記述されていません）。

### void ofApp::dragEvent(ofDragInfo dragInfo)

- ドラッグアンドドロップイベントが発生したときに呼び出される関数（コードには具体的な処理が記述されていません）。

## ofApp.h