# Android Hilt 项目环境设置与构建指南

## 环境要求

- Android Studio 2022.3.1 或更高版本
- JDK 11（必须使用 JDK 11，不支持更高版本）
- Gradle 7.3.0
- Android SDK 33
- Kotlin 1.7.20
- Hilt 2.40.1
- AndroidX 支持库

## 环境设置步骤

1. 安装 JDK 11

   ```bash
   # 使用 Homebrew 安装 JDK 11
   brew install openjdk@11
   
   # 配置 JAVA_HOME 环境变量
   echo 'export JAVA_HOME=$(/usr/libexec/java_home -v 11)' >> ~/.zshrc
   source ~/.zshrc
   ```

2. 安装 Android Studio
   - 从 [Android Studio 官网](https://developer.android.com/studio) 下载并安装最新版本
   - 启动 Android Studio，完成初始设置向导
   - 安装 Android SDK Platform 33 和相应的构建工具

3. 配置 Android SDK
   - 打开 Android Studio
   - 进入 Preferences > Appearance & Behavior > System Settings > Android SDK
   - 确保安装了以下组件：
     - Android SDK Platform 33
     - Android SDK Build-Tools
     - Android Emulator
     - Android SDK Platform-Tools

## 项目构建与安装

1. 克隆项目

   ```bash
   git clone https://github.com/googlecodelabs/android-hilt.git
   cd android-hilt
   ```

2. 设置必要的环境变量

   ```bash
   # 设置 Android SDK 路径
   export ANDROID_HOME=$HOME/Library/Android/sdk
   
   # 设置 JDK 11 路径
   export JAVA_HOME=$(/usr/libexec/java_home -v 11)
   ```

3. 使用 Gradle 构建项目

   ```bash
   # 清理并构建项目
   ./gradlew clean build
   
   # 注意：构建过程中可能会出现 lint 错误，这些错误主要是代码质量检查，不影响应用运行
   # 如果需要忽略这些错误，可以在 app/build.gradle 中添加：
   # android {
   #     lint {
   #         baseline = file("lint-baseline.xml")
   #         checkReleaseBuilds = false
   #         abortOnError = false
   #     }
   # }
   ```

4. 检查模拟器状态

   ```bash
   # 列出已连接的设备
   adb devices
   
   # 如果模拟器未运行，启动模拟器
   ~/Library/Android/sdk/emulator/emulator -list-avds
   ~/Library/Android/sdk/emulator/emulator -avd YOUR_AVD_NAME
   ```

5. 安装应用

   ```bash
   # 安装 debug 版本到模拟器
   ./gradlew installDebug
   ```

6. 启动应用

   ```bash
   # 启动 MainActivity
   adb shell am start -n com.example.android.hilt/.ui.MainActivity
   ```

## 常见问题解决

1. 如果遇到 Java 版本不兼容问题：
   - 确保使用 JDK 11（不要使用更高版本）
   - 检查 JAVA_HOME 环境变量设置：`echo $JAVA_HOME`
   - 在 Android Studio 中设置正确的 JDK 路径

2. 如果遇到 Gradle 构建失败：
   - 尝试清理项目：`./gradlew clean`
   - 删除 .gradle 缓存目录：`rm -rf .gradle`
   - 重新同步项目：`./gradlew --refresh-dependencies`
   - 检查 Gradle 版本兼容性

3. 如果遇到模拟器问题：
   - 确保模拟器正在运行：`adb devices`
   - 检查模拟器状态：`adb shell getprop sys.boot_completed`
   - 重启 adb 服务：`adb kill-server && adb start-server`
   - 如果模拟器无响应，尝试重启模拟器

4. 如果遇到 Hilt 相关错误：
   - 确保项目正确应用了 Hilt 插件
   - 检查 Hilt 版本是否与项目配置匹配
   - 验证所有必要的 Hilt 注解是否正确使用

5. 如果遇到 lint 错误：
   - 这些错误主要是代码质量检查，不影响应用运行
   - 如果需要忽略这些错误，可以在 app/build.gradle 中添加：

     ```groovy
     android {
         lint {
             baseline = file("lint-baseline.xml")
             // 或者完全禁用 lint
             checkReleaseBuilds = false
             abortOnError = false
         }
     }
     ```

## 参考资源

- [Android Studio 下载](https://developer.android.com/studio)
- [JDK 11 下载](https://adoptium.net/temurin/releases/?version=11)
- [Gradle 用户指南](https://docs.gradle.org/current/userguide/userguide.html)
- [Hilt 官方文档](https://dagger.dev/hilt/)
- [AndroidX 迁移指南](https://developer.android.com/jetpack/androidx/migrate)
