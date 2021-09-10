# InkscapeでAndroid/iOSアプリの共通アイコンを作成するテンプレート

[この記事](https://www.xda-developers.com/graphic-design-for-android-developers-creating-the-ideal-app-icon/)で紹介されていたInkscape用のテンプレートに若干の修正を加えつつ、Android/iOS用のアイコン画像を生成する手順をまとめた。

## 使い方

1. [Inkscape](https://inkscape.org/ja/) で [adaptive_icon_template.svg](adaptive_icon_template.svg) を開く
1. `Background`レイヤーに、キャンバスいっぱいの矩形を描画し背景色を塗るか、背景画像を配置する
1. `Foreground`レイヤーに、[Android](https://material.io/design/iconography/product-icons.html#design-principles) および [iOS](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/app-icon/) のガイドラインを参考にしながらアイコンをデザインする
1. Android用に最大サイズのアイコンを生成する
    1. `Guidelines`レイヤーと`Mask`レイヤーを非表示にする
    1. `PNG画像にエクスポート`を開く
    1. エクスポート領域を`ページ`、画像サイズを`512px`四方にして、`icon_512.png`にエクスポート
1. (任意) Android用の[アダプティブアイコン](https://developer.android.com/guide/practices/ui_guidelines/icon_design_adaptive?hl=ja)用のアイコンを生成する
    1. `Foreground`レイヤーを非表示にする
    1. エクスポート領域を`ページ`、画像サイズを`432px`四方にして、`background_432.png`にエクスポート
    1. `Foreground`レイヤーを表示し、`Background`レイヤーを非表示にする
    1. エクスポート領域を`ページ`、画像サイズを`432px`四方にして、`foreground_432.png`にエクスポート

1. iOS用に最大サイズのアイコンを生成する
    1. `Mask`レイヤーを非表示にする
    1. `Guidelines`の矩形ガイド線を選択
    1. `Guidelines`レイヤーを非表示にする
    1. `PNG画像にエクスポート`を開く
    1. エクスポート領域を`選択範囲`、画像サイズを`1024px`四方にして、`icon_1024.png`にエクスポート

## FlutterでAndroid/iOS用のアイコンを自動生成する

[Flutter](https://flutter.dev/)でアプリを作成している場合、[flutter\_launcher\_icons](https://pub.dev/packages/flutter_launcher_icons)を使用することで、予め用意しておいた画像から、Android/iOS用のアイコンを自動生成することができる、

1. 生成したアイコンを `assets/icons/` 配下に配置
1. `pubspec.yaml`を以下のように修正

    ```yaml
    dev_dependencies:
      flutter_test:
        sdk: flutter
      # 以下を追加
      # 必要に応じて ^0.9.2 のようにバージョンを指定する
      flutter_launcher_icons:
    
    # 以下をファイル末尾に追加
    flutter_icons:
      ios: true
      android: true
      remove_alpha_ios: true
      image_path_ios: "assets/icons/icon_1024.png"
      image_path_android: "assets/icons/icon_512.png"
      # 以下はAndroidのアダプティブアイコン対応の場合のみ
      adaptive_icon_foreground: "assets/icons/foreground_432.png"
      adaptive_icon_background: "assets/icons/background_432.png"
    ```

1. 以下のコマンドを実行することで、各PF向けのアイコンが生成される

    ```bash
    flutter pub pub run flutter_launcher_icons:main
    ```

## 参考

* Android
    * [Product icons \- Material Design](https://material.io/design/iconography/product-icons.html#design-principles)
    * [アダプティブ アイコン  \|  Android デベロッパー  \|  Android Developers](https://developer.android.com/guide/practices/ui_guidelines/icon_design_adaptive?hl=ja)
* iOS
    * [App Icon \- Icons and Images \- iOS \- Human Interface Guidelines \- Apple Developer](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/app-icon/)
* [Graphic design for Android developers: Creating the ideal app icon](https://www.xda-developers.com/graphic-design-for-android-developers-creating-the-ideal-app-icon/)
* [【Flutter】アプリアイコンをiOSとAndroid用に同時に一括生成する｜Flutterラボ｜note](https://note.com/hatchoutschool/n/naaecb106670f)
