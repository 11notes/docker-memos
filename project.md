${{ image: Dashboard.png }}

${{ content_synopsis }} This image will run memos [rootless](https://github.com/11notes/RTFM/blob/main/linux/container/image/rootless.md) and [distroless](https://github.com/11notes/RTFM/blob/main/linux/container/image/distroless.md), for maximum security and performance. For infos on how to setup memos with OIDC consult their [documentation](https://www.usememos.com/docs/advanced-settings/sso).

${{ content_uvp }} Good question! Because ...

${{ github:> [!IMPORTANT] }}
${{ github:> }}* ... this image runs [rootless](https://github.com/11notes/RTFM/blob/main/linux/container/image/rootless.md) as 1000:1000
${{ github:> }}* ... this image has no shell since it is [distroless](https://github.com/11notes/RTFM/blob/main/linux/container/image/distroless.md)
${{ github:> }}* ... this image is auto updated to the latest version via CI/CD
${{ github:> }}* ... this image has a health check
${{ github:> }}* ... this image runs read-only
${{ github:> }}* ... this image is automatically scanned for CVEs before and after publishing
${{ github:> }}* ... this image is created via a secure and pinned CI/CD process
${{ github:> }}* ... this image is very small

If you value security, simplicity and optimizations to the extreme, then this image might be for you.

${{ content_comparison }}

${{ title_volumes }}
* **${{ json_root }}/var** - Directory of all uploaded data

${{ content_compose }}

${{ content_defaults }}

${{ content_environment }}
| `MEMOS_MODE` | Mode of operation (prod, dev or demo) | prod |
| `MEMOS_ADDR` | IP to listen on | 0.0.0.0 |
| `MEMOS_PORT` | Port to listen on | 5230 |
| `MEMOS_DATA` | Data directory to store files | /memos/var |
| `MEMOS_DRIVER` | Backend driver to use (postgres, sqlite and mysql) | postgres |

${{ content_source }}

${{ content_parent }}

${{ content_built }}

${{ content_tips }}