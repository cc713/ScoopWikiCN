Auto Update 是为包维护者提供的工具. 它会自动检查应用程序的新版本, 并相应地更新清单. 它有助于消除更新清单的许多繁琐工作, 同时降低人为错误的风险.

这里你可以找到应用清单 `autoupdate` 部分工作的深入解释.

# 使用 `checkver.ps1` 查询和自动更新

用 `checkver.ps1` 查询 bucket 中特定应用或所有应用的当前版本. 如果更新版本存在, 你也可以用 `checkver.ps1` 自动更新清单.

## 查询当前版本

打开 PowerShell/cmd, 然后 `cd` 到 **scoop** 根目录 (`apps\scoop\current`) 或 buckets 存储库目录, 并运行以下命令.

要查询 bucket 中特定应用的当前版本, 运行:

```powershell
.\bin\checkver.ps1 <app>
# or .\bin\checkver.ps1 -App <app>
```

要查询 bucket 中所有应用的当前版本, 运行:

```powershell
.\bin\checkver.ps1 *
# or .\bin\checkver.ps1 -App *
```

## 更新清单

在 `checkver.ps1` 的输出中, 你可以看到过时的应用是否有自动更新. 如果是, 你可以运行以下命令自动更新相应应用的清单 (使用 `*` 会更新所有应用)

```powershell
.\bin\checkver.ps1 <app> -u
# or .\bin\checkver.ps1 -App <app> -Update
```

要查询不在 `bucket` 目录中的应用 (例如 `.`, `.\TODO`, 等等), 指定目录作为 `checkver.ps1` 的第二参数

```powershell
.\bin\checkver.ps1 <app> <dir> -u
# or .\bin\checkver.ps1 -App <app> -Dir <dir> -Update
```

推荐验证更新后清单是否有效, 使用以下命令安装应用

```powershell
scoop install bucket\<app>.json
```

## `checkver.ps1` 的参数

- `-App` (`-a APP`)
  - 要搜索的清单名.
  - 支持占位符 (`*`).
- `-Dir` (`-d DIR`)
  - 搜索清单的位置.
- `-Update` (`-u`)
  - 更新给定的清单.
- `-ForceUpdate` (`-f`)
  - 检查清单并更新, 即使没有新版本.
- `-SkipUpdated` (`-s`)
  - 检查给定清单, 只列出过时的清单.
- `-Version` (`-v VER`)
  - 使用给定的版本 `Ver` 检查给定清单.
  - 通常使用 `-u` 更新到特定版本.

# 添加 `checkver` 到清单

## 在 `checkver` 中使用正则表达式

最简单的解决方案是用正则表达式, 它会匹配 `homepage` 的源代码. 例子: [go](https://github.com/ScoopInstaller/Main/blob/master/bucket/go.json)

```json
"homepage": "<https://golang.org>",
"checkver": "Build version go([\\d\\.]+)\\."
```

如果你不熟悉正则表达式或想测试你的正则表达式是否匹配正确文本, 可以使用在线工具 ([RegEx101](https://regex101.com/) 或 [RegExr](https://regexr.com/)).

如果 `homepage` 不包含 version/版本, 使用其它 url. 例子: [7zip](https://github.com/ScoopInstaller/Main/blob/master/bucket/7zip.json)

```json
"homepage": "https://www.7-zip.org/",
"checkver": {
    "url": "https://www.7-zip.org/download.html",
    "regex": "Download 7-Zip ([\\d.]+)"
}
```

## 在 `checkver` 中使用 JSONPath

使用 JSON 端点配合 [JSONPath 表达式](https://goessner.net/articles/JsonPath/) 获取版本. 可以使用点标记或括号标记. 例子: [mro](https://github.com/ScoopInstaller/Main/blob/master/bucket/mro.json)

```json
"checkver": {
    "url": "https://mran.microsoft.com/assets/configurations/app.config.json",
    "jp": "$.latestMicrosoftRVersion"
}
```

如果你不熟悉 JSONPath 或想测试你的 JSONPath 是否匹配正确文本, 你可以使用在线工具 ([JSONPath Expression Tester](https://jsonpath.curiousconcept.com/)).

`checkver.jsonpath` 中可以有 JSONPath 查询, 也可以有正则表达式 ([样本参考](https://www.newtonsoft.com/json/help/html/RegexQuery.htm)). 例子: [nuget](https://github.com/ScoopInstaller/Main/blob/master/bucket/nuget.json)

```json
"checkver": {
    "url": "https://dist.nuget.org/index.json",
    "jp": "$..versions[?(@.displayName == 'nuget.exe - recommended latest')].version"
    }
```

## 在 `checkver` 中同时使用正则表达式和 JSONPath

如果 `checkver.regex` 和 `checkver.jsonpath` 都被赋值, **scoop** 用 `checkver.jsonpath` 提取匹配 `checkver.regex` 的字符串查找版本. 例子: [nwjs](https://github.com/lukesampson/scoop-extras/blob/master/bucket/nwjs.json)

```json
"checkver": {
    "url": "https://nwjs.io/versions.json",
    "jsonpath": "$.stable",
    "regex": "v([\\d.]+)"
}
```

## 特殊情况

通过设置 `checkver` 为 `github`, `homepage` 为存储库 URL, 使用最新的 app release. 这会尝试用 `\/releases\/tag\/(?:v|V)?([\d.]+)` 匹配 tag/标签. *应用作者必须使用 Github 的 Release 功能才能正常工作. 预发布将被忽略!* 例子: [nvm](https://github.com/ScoopInstaller/Main/blob/master/bucket/nvm.json)

```json
"homepage": "https://github.com/coreybutler/nvm-windows",
"checkver": "github"
```

或为 homepage 和 repository 使用不同 url. 例子: [cmder](https://github.com/ScoopInstaller/Main/blob/master/bucket/cmder.json)

```json
"homepage": "http://cmder.net",
"checkver": {
    "github": "https://github.com/cmderdev/cmder"
}
```

用 `checkver.reverse: true` 让 `checkver.regex` 匹配找到的最后一个匹配项 (默认匹配第一个). 例子: [x264](https://github.com/ScoopInstaller/Main/blob/master/bucket/x264.json)

```json
"checkver": {
    "url": "https://download.videolan.org/pub/videolan/x264/binaries/win64/",
    "re": "x264-r(?<version>[\\d]+)-(?<commit>[a-fA-F0-9]{7}).exe",
    "reverse": true
}
```

在 `checkver.regex` 中使用捕获组会让 [捕获变量](#captured-variables) 可用于 `checkver.replace` 匹配复杂版本 或 [`autoupdate`](#adding-autoupdate-to-a-manifest) 属性.

这个例子会提供 `$matchVersion` 和 `$matchShort` 作为变量 (用于 `autoupdate`). 例子: [git](https://github.com/ScoopInstaller/Main/blob/master/bucket/git.json)

```json
"checkver": {
    "url": "https://github.com/git-for-windows/git/releases/latest",
    "re": "v(?<version>[\\d\\w.]+)/PortableGit-(?<short>[\\d.]+).*\\.exe"
}
```

这个例子会提供 `${1}, ${2}, ${3}` (用于 `checkver.replace`) 和 `$matchSha` (用于 `autoupdate`) 作为变量. 例子: [pshazz](https://github.com/ScoopInstaller/Main/blob/master/bucket/pshazz.json)

```json
"checkver": {
    "url": "https://github.com/lukesampson/pshazz/commits/master.atom",
    "re": "(\\d+)-(\\d+)-(\\d+)[\\S\\s]*?(?<sha>[0-9a-f]{40})",
    "replace": "0.${1}.${2}.${3}"
}
```

## `checkver` 的属性

- `checkver`: "regex". RegEx for finding the version on the `homepage`
  - `github`: "uri". URL to the app's Github repository
  - `url`: "uri". Page where the version can be found
    - Supports [version variables](#version-variables)
  - `regex|re`: "regex". RegEx for finding the version
  - `jsonpath|jp`: "jsonpath". JSONPath expression for finding the version
  - `xpath`: "string". XPath expression for finding the version
  - `reverse`: "boolean". If or not match the last occurrence found
  - `replace`: "string". Replace the matched value with a calculated value
    - Supports [captured variables](#captured-variables)
  - `useragent`: "string". User-Agent that used to get webpage content (only used in [fiddler](https://github.com/lukesampson/scoop-extras/blob/master/bucket/fiddler.json))
    - Supports [version variables](#version-variables)

# 添加 `autoupdate` 到清单

要让自动更新功能工作, 需要 [`checkver`](#adding-checkver-to-a-manifest) 属性找到最新版本号.

```json
"autoupdate": {
    "note": "Thanks for using autoupdate, please test your updates!",
    "architecture": {
        "64bit": {
            "url": "https://example.org/dl/example-v$version-x64.msi"
        },
        "32bit": {
            "url": "https://example.org/dl/example-v$version-x86.msi"
        }
    }
}
```

一些使用 `autoupdate` 功能的示例清单:

- [nodejs](https://github.com/ScoopInstaller/Main/blob/master/bucket/nodejs.json)

```json
"autoupdate": {
    "architecture": {
        "64bit": {
            "url": "https://nodejs.org/dist/v$version/node-v$version-win-x64.7z",
            "extract_dir": "node-v$version-win-x64"
        },
        "32bit": {
            "url": "https://nodejs.org/dist/v$version/node-v$version-win-x86.7z",
            "extract_dir": "node-v$version-win-x86"
        }
    },
    "hash": {
        "url": "$baseurl/SHASUMS256.txt.asc"
    }
}
```

- [php](https://github.com/ScoopInstaller/Main/blob/master/bucket/php.json)

```json
"autoupdate": {
    "architecture": {
        "64bit": {
            "url": "https://windows.php.net/downloads/releases/php-$version-Win32-VC15-x64.zip"
        },
        "32bit": {
            "url": "https://windows.php.net/downloads/releases/php-$version-Win32-VC15-x86.zip"
        }
    },
    "hash": {
        "url": "$baseurl/sha256sum.txt"
    }
}
```

- [nginx](https://github.com/ScoopInstaller/Main/blob/master/bucket/nginx.json)

```json
"autoupdate": {
    "url": "https://nginx.org/download/nginx-$version.zip",
    "extract_dir": "nginx-$version"
}
```

- [imagemagick](https://github.com/ScoopInstaller/Main/blob/master/bucket/imagemagick.json)

```json
"autoupdate": {
    "architecture": {
        "64bit": {
            "url": "https://www.imagemagick.org/download/binaries/ImageMagick-$version-portable-Q16-x64.zip"
        },
        "32bit": {
            "url": "https://www.imagemagick.org/download/binaries/ImageMagick-$version-portable-Q16-x86.zip"
        }
    },
    "hash": {
        "mode": "rdf",
        "url": "https://www.imagemagick.org/download/binaries/digest.rdf"
    }
}
```

Some examples using the `autoupdate` feature with [captured variables](#captured-variables) or [version variables](#version-variables):

- [openjdk](https://github.com/ScoopInstaller/Java/blob/master/bucket/openjdk.json)

```json
"checkver": {
    "re": "/(?<type>early_access|GA)/(?<path>jdk(?<major>[\\d.]+)(?:.*)?/(?<build>[\\d]+)(?:/GPL|/binaries)?)/(?<file>openjdk-(?<version>[\\d.]+)(?<ea>-ea)?(?:\\+[\\d]+)?_windows-x64_bin.(zip|tar.gz))",
    "replace": "${version}-${build}${ea}"
},
"autoupdate": {
    "architecture": {
        "64bit": {
            "url": "https://download.java.net/java/$matchType/$matchPath/$matchFile"
        }
    },
    "hash": {
        "url": "$url.sha256"
    },
    "extract_dir": "jdk-$matchVersion"
}
```

- [mysql](https://github.com/ScoopInstaller/Main/blob/master/bucket/mysql.json)

```json
"autoupdate": {
    "architecture": {
        "64bit": {
            "url": "https://dev.mysql.com/get/Downloads/MySQL-$majorVersion.$minorVersion/mysql-$version-winx64.zip",
            "extract_dir": "mysql-$version-winx64",
            "hash": {
                "url": "https://dev.mysql.com/downloads/mysql/",
                "find": "md5\">([A-Fa-f\\d]{32})"
            }
        }
    }
}
```

## `autoupdate` 的属性

Most of the [manifest properties](https://github.com/ScoopInstaller/Scoop/wiki/App-Manifests) could be added into `autoupdate`: `bin`, `extract_dir`, `extract_to`, `env_add_path`, `env_set`, `installer`, `license`, `note`, `persist`, `post_install`, `psmodule`, `shortcuts`, and the most important ones, `url` and `hash`.

All the properties except `autoupdate.note` can be set globally for all architectures or for each architecture separately (under `architecture.64bit` or `architecture.32bit`). Global properties can be used to update each architectural properties, i.e., if only setted globally, `autoupdate.url` is used to update either `architecture.64bit.url` or `architecture.32bit.url`.

All the properties except `hash` support [captured variables](#captured-variables) and [version variables](#version-variables), and `hash` has its own [property](#adding-hash-to-autoupdate) for obtaining hash values without download the actual files.

# 添加 `hash` 到 `autoupdate`

There are several ways to obtain the hash of the new file. If the app provider publishes hash values it is possible to extract these from their website or hashfile. If nothing is defined or something goes wrong while getting the hash values the target files will be downloaded and hashed locally.

Hash value can be directly extracted by the following method (`autoupdate.hash.mode`):

- Using RegEx for plain text file or webpage (`extract`, *predefined `fosshub`, `sourceforge`*)
- Using JSONPath for JSON file (`json`)
- Using XPath for XML file (`xpath`)
- Using Digest for RDF file (`rdf`)
- Using download header or `.meta4` for [Metalink](http://www.metalinker.org) (`metalink`)

## 在 `hash.url` 中指定 url

`url` in `hash` property accepts URL with [captured variables](#captured-variables), [version variables](#version-variables) or [URL variables](#url-variables).

- Use [captured variables](#captured-variables). Example: [qemu](https://github.com/ScoopInstaller/Main/blob/master/bucket/qemu.json)

```json
"checkver": {
    "re": "<strong>(?<year>\\d{4})-(?<month>\\d{2})-(?<day>\\d{2})</strong>: New QEMU installers \\((?<version>[\\d.a-z\\-]+)\\)"
},
"autoupdate": {
    "architecture": {
        "64bit": {
            "url": "https://qemu.weilnetz.de/w64/qemu-w64-setup-$matchYear$matchMonth$matchDay.exe#/dl.7z",
            "hash": {
                "url": "https://qemu.weilnetz.de/w64/qemu-w64-setup-$matchYear$matchMonth$matchDay.sha512"
            }
        },
        "32bit": {
            "url": "https://qemu.weilnetz.de/w32/qemu-w32-setup-$matchYear$matchMonth$matchDay.exe#/dl.7z",
            "hash": {
                "url": "https://qemu.weilnetz.de/w32/qemu-w32-setup-$matchYear$matchMonth$matchDay.sha512"
            }
        }
    }
}
```

- Use [version variables](#version-variables). Example: [julia](https://github.com/ScoopInstaller/Main/blob/master/bucket/julia.json)

```json
"hash": {
    "url": "https://julialang-s3.julialang.org/bin/checksums/julia-$version.sha256"
}
```

- Use [URL variables](#url-variables) and append suffix to it. Example: [apache](https://github.com/ScoopInstaller/Main/blob/master/bucket/apache.json)

```json
"hash": {
    "url": "$url_fossies.sha256"
}
```

## 从纯文本文件或网页获取 hash

如果应用提供者公开 hash value/哈希值为纯文本文件或在某些网页上, `checkver.ps1` 可以通过使用合适的正则表达式提取它.

### 在 `autoupdate.hash` 使用内置正则表达式

有一些 *默认* 正则表达式内置于 **scoop**, 例如, `^([a-fA-F0-9]+)$` 和 `([a-fA-F0-9]{32,128})[\x20\t]+.*$basename(?:[\x20\t]+\d+)?`.

```powershell
# ^([a-fA-F0-9]+)$
abcdef0123456789abcdef0123456789abcdef01
# ([a-fA-F0-9]{32,128})[\x20\t]+.*`$basename(?:[\x20\t]+\d+)?
abcdef0123456789abcdef0123456789abcdef01 *example.zip
```

例子见 [上](#在-hash.url-中指定-url).

### 在 `autoupdate.hash` 中使用自定义正则表达式

如果内置正则表达式不适用, 自定义正则表达式可用于提取 hash value/哈希值. [Hash 变量](#hash-variables) (`$md5`, `$sha1`, `$sha256`, 等等) 可用于简化表达式.

- [curl](https://github.com/ScoopInstaller/Main/blob/master/bucket/curl.json)

```json
"hash": {
    "url": "$baseurl/hashes.txt",
    "find": "SHA256\\($basename\\)=\\s+([a-fA-F\\d]{64})"
}
```

- [python](https://github.com/ScoopInstaller/Main/blob/master/bucket/python.json)

```json
"hash": {
    "url": "https://www.python.org/downloads/release/python-$cleanVersion/",
    "find": "$basename[\\S\\s]+?$md5"
}
```

### FossHub 和 SourceForge 的特殊情况

有两种预定义的特殊情况: [FossHub](https://www.fosshub.com) 和 [SourceForge](https://sourceforge.net).

- FossHub
  - URL 格式: `^(?:.*fosshub.com\/).*(?:\/|\?dwl=)(?<filename>.*)$`
    - `https://www.fosshub.com/Calibre.html?dwl=calibre-64bit-3.44.0.msi`
    - `https://www.fosshub.com/Calibre.html/calibre-64bit-3.44.0.msi`
  - RegEx: `$basename.*?"sha256":"([a-fA-F0-9]{64})"`
- SourceForge
  - URL 格式: `(?:downloads\.)?sourceforge.net\/projects?\/(?<project>[^\/]+)\/(?:files\/)?(?<file>.*)`
    - `https://downloads.sourceforge.net/project/nsis/NSIS%203/3.04/nsis-3.04.zip`
    - `https://sourceforge.net/projects/nsis/files/NSIS%203/3.04/nsis-3.04.zip`
  - RegEx: `"$basename":.*?"sha1":\s"([a-fA-F0-9]{40})"`

对于匹配以上格式的 `autoupdate.url`, hash 模式自动设置为 `fosshub` 或 `sourceforge` 而无需 `hash` 属性.

- [calibre](https://github.com/ScoopInstaller/Main/blob/master/bucket/curl.json)

```json
"autoupdate": {
    "url": "https://www.fosshub.com/Calibre.html/calibre-portable-installer-$version.exe"
}
```

- [nsis](https://github.com/lukesampson/scoop-extras/blob/master/bucket/nsis.json)

```json
"autoupdate": {
    "url": "https://downloads.sourceforge.net/project/nsis/NSIS%20$majorVersion/$version/nsis-$version.zip",
    "extract_dir": "nsis-$version"
}
```

## 从 JSON 文件获取 hash

For JSON file, use a JSON endpoint with [JSONPath expressions](https://goessner.net/articles/JsonPath/) to retrieve the hash. Either dot-notation or bracket-notation can be used. Example: [openssl](https://github.com/ScoopInstaller/Main/blob/master/bucket/openssl.json)

```json
"hash": {
    "mode": "json",
    "jp": "$.files.['$basename'].sha512",
    "url": "$baseurl/win32_openssl_hashes.json"
}
```

There could be a JSONPath query in `autoupdate.hash.jsonpath`, and so does RegEx ([sample reference](https://www.newtonsoft.com/json/help/html/RegexQuery.htm)). Example: [mro](https://github.com/ScoopInstaller/Main/blob/master/bucket/mro.json)

```json
"hash": {
    "mode": "json",
    "jsonpath": "$..versions[?(@.downloadText == '$basename')].sha256",
    "url": "https://mranapi.azurewebsites.net/api/download"
}
```

## 从 XML 文件获取 hash

Use XPath to retrieve the hash from a XML file. Example: [googlechrome](https://github.com/h404bi/dorado/blob/master/bucket/googlechrome.json)

```json
"hash": {
    "url": "https://lab.skk.moe/chrome/api/chrome.min.xml",
    "xpath": "/chromechecker/stable64[version=\"$version\"]/sha256"
}
```

## 从 RDF 文件获取 hash

[Resource Description Framework (RDF)](https://www.w3.org/TR/rdf11-concepts/) is a framework for representing information in the Web. Hash value could be extracted from RDF file. Example: [imagemagick](https://github.com/ScoopInstaller/Main/blob/master/bucket/imagemagick.json)

```json
"hash": {
    "mode": "rdf",
    "url": "https://www.imagemagick.org/download/binaries/digest.rdf"
}
```

## 从 Metalink 获取 hash

[Metalink](http://www.metalinker.org/) is an Internet standard that harnesses the speed and power of peer to peer networking and traditional downloads with a single click. For download URL that supported Metalink, hash value could be retrieved from download URL's header or a `.meta4` file. Example: [libreoffice-fresh](https://github.com/lukesampson/scoop-extras/blob/master/bucket/libreoffice-fresh.json)

```json
"hash": {
    "mode": "metalink"
}
```

## `autoupdate.hash` 的属性

可以为所有架构全局设置所有属性, 也可以为每个架构单独设置所有属性

- `mode`: "string|enum".
  - **`extract`**: 用正则表达式从纯文本文件或网页提取 (默认, 可省略)
  - `json`: 用 JSONPath 从 JSON 文件提取
  - `xpath`: 用 XPath 从 XML 文件提取
  - `rdf`: 从 RDF 文件提取
  - `metalink`: 从 Metalink 的 header/头 或 `.meta4` 文件提取
  - `fosshub`: *自动*. 为 FossHub 预定义
  - `sourceforge`: *自动*. 为 SourceForge 预定义
  - `download`: 下载应用在本地计算 hash (备用)
- `url`: "uri". URL 模板, 用于下载 RDF/JSON 文件或提取 hash
  - 支持 [captured variables](#captured-variables)
  - 支持 [version variables](#version-variables)
  - 支持 [URL variables](#url-variables)
- `regex|find`: "regex". RegEx expression to extract the hash
  - 支持: `^([a-fA-F0-9]+)$` and `([a-fA-F0-9]{32,128})[\x20\t]+.*$basename(?:[\x20\t]+\d+)?`
  - 支持 [captured variables](#captured-variables)
  - 支持 [version Variables](#version-variables)
  - 支持 [URL variables](#url-variables)
  - 支持 [hash variables](#hash-variables)
- `jsonpath|jp`: "jsonpath". JSONPath expression to extract the hash
  - 支持 [captured variables](#captured-variables)
  - 支持 [version Variables](#version-variables)
  - 支持 [URL variables](#url-variables)
- `xpath`: "string". XPath expression to extract the hash
  - 支持 [captured variables](#captured-variables)
  - 支持 [version Variables](#version-variables)
  - 支持 [URL variables](#url-variables)
- *`type`: "string|enum". 已废弃, hash 类型自动确定*

# 内部可替换变量

## 捕获变量

- 用于 `checkver.replace`
  - `${1}`, `${2}`, `${3}`...: 未命名组
  - `${name1}`, `${Name2}`, `${NAME3}`...: 命名组
    - `(?<name1>...)`, `(?<Name2>...)`, `(?<NAME3>...)`...
- 用于 `autoupdate`
  - `$match1`, `$match2`, `$match3`...: 未命名组
  - `$matchName1`, `$matchName2`, `$matchName3`...: 命名组
    - `(?<name1>...)`, `(?<Name2>...)`, `(?<NAME3>...)`...
    - *注意变量名中唯一的大写字符*

## Version 变量

- `$version`: `3.7.1`
- `$underscoreVersion`: `3_7_1`
- `$dashVersion`: `3-7-1`
- `$cleanVersion`: `371`
- `$version` (例如 `3.7.1.2`) 被每个 `.` 分割, 赋值给:
  - `$majorVersion`: `3`
  - `$minorVersion`: `7`
  - `$patchVersion`: `1`
  - `$buildVersion`: `2`
- `$matchHead`: Returns first two or three digits seperated by a dot (e.g. `3.7.1-rc.1` = `3.7.1` , `3.7.1.2-rc.1` = `3.7.1` or `3.7-rc.1` = `3.7`)
- `$matchTail`: Returns the rest of `$matchHead` (例如 `3.7.1-rc.1` = `-rc.1` , `3.7.1.2-rc.1` = `.2-rc.1` 或 `3.7-rc.1` = `-rc.1`)
- `$preReleaseVersion`: 在最后 `-` 后的所有 (例如 `3.7.1-rc.1` 中的 `rc.1`)
- Each capturing group in the [`checkver` property](#adding-checkver-to-a-manifest) adds a `$matchX` variable (named groups are allowed). Matching `v3.7.1/3.7` with [`v(?<version>[\d.]+)\/(?<short>[\d.]+)`](https://regex101.com/r/M7RP3p/1) would result in:
  - `$match1` 或 `$matchVersion`: `3.7.1`
  - `$match2` 或 `$matchShort`: `3.7`

## URL 变量

- `$url`: 没有 fragments/片段(`#/dl.7z`)的 autoupdate URL/自动更新 URL [例子 `http://example.com/path/file.exe`]
- `$baseurl`: 没有文件名和片段(`#/dl.7z`)的 autoupdate URL [e.g. `http://example.com/path`]
- `$basename`: 来自 autoupdate URL 的文件名 (忽略片段 `#/dl.7z`)

## Hash 变量

- `$md5`: `([a-fA-F0-9]{32})` MD5 hash 类型
- `$sha1`: `([a-fA-F0-9]{40})` SHA-1 hash 类型
- `$sha256`: `([a-fA-F0-9]{64})` SHA-256 hash 类型
- `$sha512`: `([a-fA-F0-9]{128})` SHA-512 hash 类型
- `$checksum`: `([a-fA-F0-9]{32,128})` MD5, SHA-1, SHA-256 或 SHA-512 hash 类型
- `$base64`: `([a-zA-Z0-9+\/=]{24,88})` BASE64 编码校验和 (可以是 MD5, SHA-1, SHA-256 或 SHA-512)

# 测试和运行 autoupdate

如果你想确认一个 autoupdate 是否工作, 例如将它添加到一个已存在的清单或创建新的, 更改 `version` 字段为更低或不同版本并运行 `checkver.ps1` 或使用 `-f` 参数.

```powershell
cd <bucket repository>
scoop config debug $true
.\bin\checkver.ps1 <app> -u
```

Check if the `url`, `hash` and `extract_dir` properties have the correct values. Try to install/uninstall the app and submit your changes.

Manifests in some known buckets are autoupdated by [ScoopInstaller/GithubActions](https://github.com/ScoopInstaller/GithubActions), so if you want some apps being autoupdated, migrate them to one of these buckets or run an instance of the excavator yourself.

- [`main`](https://github.com/ScoopInstaller/Main): 每小时更新
- [`extras`](https://github.com/ScoopInstaller/Extras): 每小时更新
- [`versions`](https://github.com/ScoopInstaller/Versions): 每日更新
- [`java`](https://github.com/ScoopInstaller/Java): 每日更新
- [`php`](https://github.com/ScoopInstaller/PHP): 每日更新
- [`games`](https://github.com/Calinou/scoop-games): 每日更新

# **scoop** status/update 示例工作流程

`scoop status` 将你已安装版本与你机器上当前 **scoop** 拷贝和 bucket 存储库比较. 如果这些没有更新, 它会输出错误版本信息. 例如:

- 已安装版本: 2.1.2
- 本地 **scoop** 版本: 2.1.3
- 线上存储库版本: 2.1.4

`scoop status` 会显示版本 2.1.3

推荐在 `scoop status` 前运行 `scoop update` (每 3 小时执行一次), 它会显示当前版本 2.1.4.

`scoop update` 只是 `git pull`s **scoop** core repo 到 `~\scoop\apps\scoop\current` 并将每个配置的 bucket 到 `~\scoop\buckets\<name>` (总之 默认 main bucket)

`bin\checkver.ps1` 仅用于维护和更新清单, 以便它们可以提交到 repo.

示例工作流程:

```powershell
cd <bucket repository>
.\bin\checkver * -u # updates all manifest in the repo/更新存储库中所有清单
git commit -m "Updated apps"
git push
scoop update
scoop status
scoop update <app>
```
