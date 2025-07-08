docker network create nacos-network

docker run -d --name mysql --network nacos-network -e MYSQL_ROOT_PASSWORD=admin -p 3306:3306 mysql:latest

docker run --name ruoyi-redis -p 6379:6379 -d redis:latest


docker run -d \
--name nacos \
--network nacos-network \
-e MODE=standalone \
-e SPRING_DATASOURCE_PLATFORM=mysql \
-e MYSQL_SERVICE_HOST=mysql \
-e MYSQL_SERVICE_DB_NAME=ry-config \
-e MYSQL_SERVICE_USER=root \
-e MYSQL_SERVICE_PASSWORD=admin \
-e NACOS_AUTH_ENABLE=true \
-e NACOS_AUTH_USERNAME=nacos \
-e NACOS_AUTH_PASSWORD=nacos \
-e NACOS_AUTH_TOKEN=$(openssl rand -base64 32) \
-e NACOS_AUTH_IDENTITY_KEY=$(openssl rand -hex 16) \
-e NACOS_AUTH_IDENTITY_VALUE=$(openssl rand -hex 16) \
-p 8848:8848 \
-p 9848:9848 \
-p 9849:9849 \
nacos/nacos-server:v2.4.0




/usr/libexec/java_home -V
查看 JAVA_HOME 和当前 Java 版本：
echo $JAVA_HOME
java -version
如果输出显示 JDK 21，说明环境变量或默认链接未切换。

**临时切换版本（仅当前终端生效）**
使用 export 直接修改 JAVA_HOME：
export JAVA_HOME=$(/usr/libexec/java_home -v 17)  # 切换到 JDK 17
java -version  # 验证版本

**永久切换JDK版本**
方法 1：修改 Shell 配置文件（推荐）
在 ~/.zshrc（或 ~/.bashrc）中添加：


    export JAVA_HOME=$(/usr/libexec/java_home -v 17)  # 默认使用 JDK 17
    export PATH=$JAVA_HOME/bin:$PATH
    
    
    然后加载配置：
    source ~/.zshrc
    
    
    方法 2：使用 jenv 管理多版本
    安装 jenv：
    brew install jenv
    jenv add /Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home
    jenv add /Library/Java/JavaVirtualMachines/jdk-21.jdk/Contents/Home
    jenv global 17  # 设置全局默认版本

**验证是否生效**
java -version  # 应显示 JDK 17
javac -version # 确保编译器版本同步