
# 小雅同步元数据

高速异步爬虫从 https://emby.xiaoya.pro/ 同步小雅元数据
##### [FAQ](爬虫FAQ.md)

---


## Docker Run

**docker-compose**

```yaml
services:
  tnnevol-emd:
    build:
      context: .
      dockerfile: Dockerfile
      args:
        - BRANCH=main
    image: tnnevol/tnnevol-emd:latest
    container_name: xiaoya-emd
    network_mode: host
    #deploy:
    #  resources:
    #    limits:
    #      memory: 1G
    volumes:
      # strm 媒体位置
      - xiaoya-media:/media
    environment:
      - TZ=Asia/Shanghai
      - CYCLE=86400
      - RESTART_AUTO_UPDATE=false
    # https://openlist.site.com 换成自己的
    command: --media /media --count 90 --paths /media/scan-paths.txt --strm-from "http://xiaoya.host:5678/d" --strm-to "https://openlist.site.com/d/xiaoya-alist"
    restart: unless-stopped
```

## Installation

#### Clone the project

```bash
  git clone https://github.com/Rik-F5/xiaoya_db
```

#### Go to the project directory

```bash
  cd xiaoya_db
```

#### Switch to venv

```bash
  python3 -m venv .venv
  source .venv/bin/activate
```

#### Install modules

```bash
  python -m pip install -r requirements.txt
```
---
## Run Locally with venv activated

```bash
python solid.py --media <Xiaoya Folder>

```
---
## Usage/Examples

Download the metadata without replace the local NFOs
```bash
python solid.py --media <folder>
```

Download the metadata and replace the local NFOs if they are out of date or different

```bash
python solid.py --media <folder> --nfo true
```

Download the metadata and save the file list to a local DB (Python3.12+)

```bash
python solid.py --media <folder> --db true
```

Do not download any files. For testing or benchmark only.

```bash
python solid.py
```
---
## Parameters

```bash
  -h, --help           show this help message and exit

  --media <folder>     Path to store downloaded media files [Default: None]

  --count [number]     Max concurrent HTTP Requests [Default: 100]

  --debug, --no-debug  Verbose debug [Default: False]

  --db, --no-db        <Python3.12+ required> Save into DB [Default: False]

  --nfo, --no-nfo      Download NFO [Default: False]

  --url [url]          Download path [Default: None]

  --purge, --no-purge  Purge removed files [Default: True]

  --all, --no-all      Download all folders [Default: False]

  --location <folder>  Path to store database files [Default: None]

  --paths <file>       Bitmap of paths or a file containing paths to be selected (See paths.example)
  --strm-from <url>    被替换的值
  --strm-to <url>      替换的值
···