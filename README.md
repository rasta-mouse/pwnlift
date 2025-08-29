# pwnlift

Named in homage to [pwndrop](https://github.com/kgretzky/pwndrop), pwnlift is a simple dotnet server application for uploading files from a desktop without the use of a C2.
Useful if you have a console access to a machine and need to take files offline for analysis (such as Code Integrity Policy files).

![pwnlift](screenshot.jpeg)

## Building

- Build the project using `dotnet publish`, specifying the target runtime that you need.  E.g `dotnet publish -r win-x64` for Windows or `-r linux-x64` for Linux.

    > If your server doesn't already have the ASP.NET runtime installed, just package it with your build using `--self-contained true`.

- Copy the `publish` folder to your server.

## Usage

Modify `appsettings.json` with the URL(s) and port(s) that you want to use.

```json
{
  "Kestrel": {
    "Endpoints": {
      "Http": {
        "Url": "http://*:8080"
      }
    }
  }
}
```

To use HTTPS, define the endpoint, the certificate path, and certificate password.

```json
{
  "Kestrel": {
    "Endpoints": {
      "Http": {
        "Url": "http://*:8080"
      },
      "Https": {
        "Url": "https://*:8443",
        "Certificate": {
          "Path": "contoso.com.pfx",
          "Password": "pass123"
        }
      }
    }
  }
}
```

    > Note that when HTTPS is enabled, the HTTP endpoint will automatically redirect to it.

Then, simply run the `pwnlift` binary.

```text
$ ./pwnlift
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: http://[::]:8080
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: https://[::]:8443
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Production
```

All uploads are dropped into the `Uploads` directory.