### manual
手册

#### Installing MkDocs
```
pip install mkdocs
```

#### Getting Started
```
# 启动服务
mkdocs serve

# 构建
mkdocs build
mkdocs build --clean
```

#### Deploy
```
git clone https://github.com/xiuery/manual.git
git pull
cd manual
mkdocs build --clean
rm -rf /home/nginx/xs-manual/*
mv dist/* /home/nginx/xs-manual
```
