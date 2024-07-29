安装sdkman(Software Development Kit Manager)
`curl -s "https://get.sdkman.io" | bash`

```
sdk install gradle 8.10
```
### 初始化 Gradle 项目

在项目目录中，使用 Gradle 初始化一个 Kotlin 项目：

bash

复制代码

`gradle init --type kotlin-application`

### 4. 修改 `build.gradle.kts` 文件

打开 `build.gradle.kts` 文件，并添加 Ktor 的依赖项。内容如下：

kotlin

复制代码

`plugins {     kotlin("jvm") version "1.9.0"     application }  group = "com.example" version = "1.0-SNAPSHOT"  repositories {     mavenCentral() }  dependencies {     implementation("io.ktor:ktor-server-core-jvm:2.3.4")     implementation("io.ktor:ktor-server-netty-jvm:2.3.4")     implementation("io.ktor:ktor-server-content-negotiation-jvm:2.3.4")     implementation("io.ktor:ktor-serialization-kotlinx-json-jvm:2.3.4")     testImplementation("io.ktor:ktor-server-tests-jvm:2.3.4")     testImplementation("org.jetbrains.kotlin:kotlin-test-junit:1.9.0") }  application {     mainClass.set("com.example.ApplicationKt") }  tasks.withType<org.jetbrains.kotlin.gradle.tasks.KotlinCompile> {     kotlinOptions.jvmTarget = "17" }`

### 5. 创建 Ktor 入口文件

在 `src/main/kotlin/com/example/` 目录下，创建一个 `Application.kt` 文件，并编写简单的 Ktor 应用代码：

kotlin

复制代码

`package com.example  import io.ktor.server.engine.* import io.ktor.server.netty.* import io.ktor.server.application.* import io.ktor.server.response.* import io.ktor.server.routing.* import io.ktor.server.plugins.contentnegotiation.* import io.ktor.serialization.kotlinx.json.*  fun main() {     embeddedServer(Netty, port = 8080) {         install(ContentNegotiation) {             json()         }         routing {             get("/") {                 call.respondText("Hello, Ktor!")             }         }     }.start(wait = true) }`

### 6. 运行项目

在项目目录下运行以下命令来启动 Ktor 服务器：

bash

复制代码

`./gradlew run`

服务器启动后，打开浏览器并访问 `http://localhost:8080`，你应该会看到 "Hello, Ktor!"。