# Flutter Quill

<p align="center" style="background-color:#282C34">
  <img src="https://user-images.githubusercontent.com/10923085/119221946-2de89000-baf2-11eb-8285-68168a78c658.png" width="600px" alt="Flutter Quill">
</p>
<h1 align="center">A rich text editor for Flutter</h1>

[![MIT License][license-badge]][license-link]
[![PRs Welcome][prs-badge]][prs-link]
[![Watch on GitHub][github-watch-badge]][github-watch-link]
[![Star on GitHub][github-star-badge]][github-star-link]
[![Watch on GitHub][github-forks-badge]][github-forks-link]

[license-badge]: https://img.shields.io/github/license/singerdmx/flutter-quill.svg?style=for-the-badge

[license-link]: ./LICENSE

[prs-badge]: https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=for-the-badge

[prs-link]: https://github.com/singerdmx/flutter-quill/issues

[github-watch-badge]: https://img.shields.io/github/watchers/singerdmx/flutter-quill.svg?style=for-the-badge&logo=github&logoColor=ffffff

[github-watch-link]: https://github.com/singerdmx/flutter-quill/watchers

[github-star-badge]: https://img.shields.io/github/stars/singerdmx/flutter-quill.svg?style=for-the-badge&logo=github&logoColor=ffffff

[github-star-link]: https://github.com/singerdmx/flutter-quill/stargazers

[github-forks-badge]: https://img.shields.io/github/forks/singerdmx/flutter-quill.svg?style=for-the-badge&logo=github&logoColor=ffffff

[github-forks-link]: https://github.com/singerdmx/flutter-quill/network/members

---

FlutterQuill is a rich text editor and a [Quill] component for [Flutter].

This library is a WYSIWYG (What You See Is What You Get) editor built
for the modern Android, iOS,
web and desktop platforms.
Check out our [Youtube Playlist] or [Code Introduction](./doc/code_introduction.md)
to take a detailed walkthrough of the code base.
You can join our [Slack Group] for discussion.

> If you are viewing this page from [pub.dev](https://pub.dev/) page, then you
> might have some issues with opening some links, open it in the GitHub repo instead.

## 📚 Table of contents

- [Flutter Quill](#flutter-quill)
  - [📚 Table of contents](#-table-of-contents)
  - [📸 Screenshots](#-screenshots)
  - [📦 Installation](#-installation)
  - [🛠 Platform Specific Configurations](#-platform-specific-configurations)
  - [🚀 Usage](#-usage)
  - [🔄 Migration](#-migration)
  - [🔤 Input / Output](#-input--output)
    - [🔗 Links](#-links)
  - [⚙️ Configurations](#️-configurations)
    - [🔗 Links](#-links-1)
    - [🖋 Font Family](#-font-family)
  - [📦 Embed Blocks](#-embed-blocks)
    - [🛠️ Using the embed blocks from `flutter_quill_extensions`](#️-using-the-embed-blocks-from-flutter_quill_extensions)
    - [🔗 Links](#-links-2)
  - [🔄 Conversion to HTML](#-conversion-to-html)
  - [🌐 Translation](#-translation)
  - [🧪 Testing](#-testing)
  - [👥 Contributors](#-contributors)

## 📸 Screenshots

<details>
<summary>Tap to show/hide screenshots</summary>

<br>

<img src="./example/assets/images/screenshot_1.png" width="250" alt="Screenshot 1">
<img src="./example/assets/images/screenshot_2.png" width="250" alt="Screenshot 2">
<img src="./example/assets/images/screenshot_3.png" width="250" alt="Screenshot 3">
<img src="./example/assets/images/screenshot_4.png" width="250" alt="Screenshot 4">

</details>

## 📦 Installation

```yaml
dependencies:
  flutter_quill: ^<latest-version-here>
```

<p align="center">OR</p>

```yaml
dependencies:
  flutter_quill:
    git: https://github.com/singerdmx/flutter-quill.git
    ref: v<latest-version-here>
```

> Using the latest version and reporting any issues you encounter on GitHub will greatly contribute to the improvement
> of the library.
> Your input and insights are valuable in shaping a stable and reliable version for all the developers. Thank you for
> being part of the open-source community!
>

## 🛠 Platform Specific Configurations

Before using the package, we must inform you the package uses the following plugins:

1. [`url_launcher`](https://pub.dev/packages/url_launcher) to open links.
2. [`device_info_plus`](https://pub.dev/packages/device_info_plus) to view info about the current device.
3. [`flutter_keyboard_visibility`](https://pub.dev/packages/flutter_keyboard_visibility) to listen for keyboard visibility
   changes.

All of them don't require any platform-specific setup.

> Starting from Flutter Quill `9.4.x`, [super_clipboard](https://pub.dev/packages/super_clipboard) has been moved
> to [FlutterQuill Extensions], to use rich text pasting, support pasting images, and gif files from external apps or websites, take a look
> at `flutter_quill_extensions` Readme.

## 🚀 Usage

First, you need to instantiate a controller

```dart
QuillController _controller = QuillController.basic();
```

And then use the `QuillEditor`, `QuillToolbar` widgets,
connect the `QuillController` to them

```dart
QuillToolbar.simple(
  configurations: QuillSimpleToolbarConfigurations(
    controller: _controller,
    sharedConfigurations: const QuillSharedConfigurations(
      locale: Locale('de'),
    ),
  ),
),
Expanded(
  child: QuillEditor.basic(
    configurations: QuillEditorConfigurations(
      controller: _controller,
      readOnly: false,
      sharedConfigurations: const QuillSharedConfigurations(
        locale: Locale('de'),
      ),
    ),
  ),
)
```

Depending on your use case, you might want to dispose of the `_controller` in `dispose` method

in most cases, it's better to.

Check out [Sample Page] for more advanced usage.

## 🔄 Migration

Starting from version `8.0.0`
We have added [Migration Guide](/doc/migration.md) for migration from different versions

## 🔤 Input / Output

This library uses [Quill Delta](https://quilljs.com/docs/delta/)
to represent the document content.
The Delta format is a compact and versatile way to describe document changes.
It consists of a series of operations, each representing an insertion, deletion,
or formatting change within the document.

Don’t be confused by its name Delta—Deltas represents both documents and changes to documents.
If you think of Deltas as the instructions for going from one document to another,
the way Deltas represents a document is by expressing the instructions starting from an empty document.

* Use `_controller.document.toDelta()` to extract the deltas.
* Use `_controller.document.toPlainText()` to extract plain text.

FlutterQuill provides some JSON serialization support so that you can save and open documents.
To save a document as JSON, do something like the following:

```dart
final json = jsonEncode(_controller.document.toDelta().toJson());
```

You can then write this to storage.

To open a FlutterQuill editor with an existing JSON representation that you've previously stored,
you can do something like this:

```dart
final json = jsonDecode(r'{"insert":"hello\n"}');

_controller.document = Document.fromJson(json);
```

### 🔗 Links

- [Quill Delta](https://quilljs.com/docs/delta/)
- [Quill Delta Formats](https://quilljs.com/docs/formats)
- [Why Quill](https://quilljs.com/guides/why-quill/)
- [Quill JS Configurations](https://quilljs.com/docs/configuration/)
- [Quill JS Interactive Playground](https://quilljs.com/playground/)
- [Quill JS GitHub repo](https://github.com/quilljs/quill)

## ⚙️ Configurations

The `QuillToolbar` and `QuillEditor` widgets let you customize a lot of things
[Sample Page] provides sample code for advanced usage and configuration.

### 🔗 Links

- [Using Custom App Widget](./doc/configurations/using_custom_app_widget.md)
- [Localizations Setup](./doc/configurations/localizations_setup.md)
- [Font Size](./doc/configurations/font_size.md)
- [Font Family](#font-family)
- [Custom Toolbar buttons](./doc/configurations/custom_buttons.md)

### 🖋 Font Family

To use your own fonts, update your [Assets](./example/assets/fonts) folder and pass in `fontFamilyValues`.
More details
on [this commit](https://github.com/singerdmx/flutter-quill/commit/71d06f6b7be1b7b6dba2ea48e09fed0d7ff8bbaa),
[this article](https://stackoverflow.com/questions/55075834/fontfamily-property-not-working-properly-in-flutter)
and [this](https://www.flutterbeads.com/change-font-family-flutter/).

## 📦 Embed Blocks

As of version 6.0, embed blocks are not provided by default as part of this package.

Instead, this package provides an interface for all the users to provide their own implementations for embed blocks.
Implementations for image, video, and
formula embed blocks are proved in a separate
package [`flutter_quill_extensions`](https://pub.dev/packages/flutter_quill_extensions).

Provide a list of embed

### 🛠️ Using the embed blocks from `flutter_quill_extensions`

To see how to use the extension package, please take a look at the [README](./flutter_quill_extensions/README.md)
of [FlutterQuill Extensions]

### 🔗 Links

- [Custom Embed Blocks](./doc/custom_embed_blocks.md)
- [Custom Toolbar](./doc/custom_toolbar.md)

## 🔄 Conversion to HTML

Having your document stored in Quill Delta format is sometimes not enough. Often you'll need to convert
it to other formats such as HTML to publish it, or send an email.

**Note**: This package supports converting from HTML back to Quill delta but it's experimental and used internally when
pasting HTML content from the clipboard to the Quill Editor

You have two options:

1. Using [quill_html_converter](./quill_html_converter/) to convert to HTML, the package can convert the Quill delta to
   HTML well
   (it uses [vsc_quill_delta_to_html](https://pub.dev/packages/vsc_quill_delta_to_html)), it is just a handy extension
   to do it more quickly
2. Another option is to use
   [vsc_quill_delta_to_html](https://pub.dev/packages/vsc_quill_delta_to_html) to convert your document
   to HTML.
   This package has full support for all Quill operations—including images, videos, formulas,
   tables, and mentions.
   Conversion can be performed in vanilla Dart (i.e., server-side or CLI) or in Flutter.
   It is a complete Dart part of the popular and
   mature [quill-delta-to-html](https://www.npmjs.com/package/quill-delta-to-html)
   Typescript/Javascript package.
   this package doesn't convert the HTML back to Quill Delta as far as we know


> **Converting to Delta from Markdown and HTML is highly experimental and shouldn't be used for production applications**, while the current implementation is far from perfect, it could improved a lot however **it will likely not work as expected**, due to differences between HTML and Delta, see this [comment](https://github.com/slab/quill/issues/1551#issuecomment-311458570) for more info.<br>
> We use it **internally** as it is more suitable for our specific use case, copying content from external websites and pasting it into the editor 
previously breaks the styles, while the current implementation is not ready, it provides a better user experience and doesn't have many downsides.

## 🌐 Translation

The package offers translations for the quill toolbar and editor, it will follow the system locale unless you set your
own locale.

Open this [page](./doc/translation.md) for more info

## 🧪 Testing

Please use [flutter_quill_test](https://pub.dev/packages/flutter_quill_test) for testing

## 👥 Contributors

- Special thanks to everyone who has contributed to this project...

<a href="https://github.com/singerdmx/flutter-quill/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=singerdmx/flutter-quill"  alt="Contributors"/>
</a>

<br>

Made with [contrib.rocks](https://contrib.rocks).

- Thanks to the [Flutter Team](https://flutter.dev/)
- Thanks to the welcoming community, the volunteers who helped along the journey, developers, contributors
  and contributors who put time and effort into everything including making all the libraries, tools, and the
  information we rely on
- We are incredibly grateful to many individuals and organizations who have played a
  role in the project.
  This includes the welcoming community, dedicated volunteers, talented developers and
  contributors, and the creators of the open-source tools we rely on.

We welcome all contributions!

Please follow these guidelines when contributing to the project.
See [CONTRIBUTING.md](./CONTRIBUTING.md) for more details.
<br>

The [CONTRIBUTING.md](./CONTRIBUTING.md) has development notes, if you're planning on contributing to the package,
please consider reading it.

[Quill]: https://quilljs.com/docs/formats

[Flutter]: https://github.com/flutter/flutter

[FlutterQuill]: https://pub.dev/packages/flutter_quill

[FlutterQuill Extensions]: https://pub.dev/packages/flutter_quill_extensions

[ReactQuill]: https://github.com/zenoamaro/react-quill

[Youtube Playlist]: https://youtube.com/playlist?list=PLbhaS_83B97vONkOAWGJrSXWX58et9zZ2

[Slack Group]: https://join.slack.com/t/bulletjournal1024/shared_invite/zt-fys7t9hi-ITVU5PGDen1rNRyCjdcQ2g

[Sample Page]: https://github.com/singerdmx/flutter-quill/blob/master/example/lib/screens/quill/quill_screen.dart
