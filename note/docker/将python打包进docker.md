# 将python的web应用打包成docker
将swagger-codegen生成的flask应用打包成docker
## Dockerfile
```
FROM python:3.8.5
MAINTAINER walkenzhong walkenzhong@gmail.com
#copy code
COPY ./app2 /service
#working directory
WORKDIR /service
#run some commend
RUN pip install -r requirements.txt
ENTRYPOINT ["python"]
CMD ["./__init__py"]
```
## 生成requirements.txt
```
pip freeze > requirements.txt
```
## build docker
将dockerfile与__init__py放在同一目录下，运行  
```
docker build -t my_service .
```
ps,最后的点不要忘记
## 运行
```
docker run -p 8000:5000 -t my_service
```
