<img src="assets/logo-long.png" alt="logo-long" style="zoom:1%;float: left;" width="500" />

![sigv4](https://github.com/arnemolland/dart-dnb/workflows/Dart%20CI/badge.svg) ![pub](https://img.shields.io/pub/v/sigv4.svg) [![style: pedantic](https://img.shields.io/badge/style-pedantic-9cf)](https://github.com/dart-lang/pedantic) ![license](https://img.shields.io/github/license/arnemolland/dart-dnb.svg)

A Dart library for signing AWS requests with Signature Version 4.

## Usage

Create a `Sigv4Client`. This will hold your secrets and configuration. Some omitted default values:

- `region` defaults to `eu-west-1`
- `serviceName`defaults to `execute-api`

```Dart
final client = Sigv4Client(
  accessKey: 'your_access_key',
  secretKey: 'your_secret_key',
);
```

The easier way to create a request is by getting a [`package:http`](https://pub.dev/packages/http) request object:

```dart
// A simple GET-request
final request = client.request('https://service.aws.com/endpoint');

get(request.url, headers: request.headers);

// A larger request
final request = client.request(
  'https://service.aws.com/endpoint',
  method: 'POST',
  queryParameters: {'key': 'value'},
  headers: {'header': 'value'},
  body: {'content': 'some-content'},
);

post(request.url, headers: request.headers, body: request.body);
```

Alternatively, you can get the canonical string and signed headers separately:

```dart
final path = 'https://service.aws.com/endpoint';
final queryParameters = {'key': 'value'};

final url = client.canonicalUrl(path, queryParameters: queryParameters);
final headers = client.signedHeaders(
  path,
  queryParameters: queryParameters,
);

get(url, headers: headers);
```
