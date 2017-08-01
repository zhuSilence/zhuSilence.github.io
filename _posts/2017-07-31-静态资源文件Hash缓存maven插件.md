---
layout: post
title: 静态资源文件Hash缓存maven插件
date: 2017-07-31
tag: Maven插件 静态资源Hash
---

### 背景
在开发的过程中，浏览器会有缓存，每次发布的新的代码，js和css都得不到及时的更新，必须清除缓存才能正常使用。关于前端页面优化的功能网上有很多介绍，不管是更新文件的版本号还是对文件进行Hash，本质上都是更改文件名，让浏览器重新下载文件。

很多前端构建工具都支持这个功能，但是对于部分前后端代码并没有分离的项目来说，并不是很好实现。这里提供一个较好的解决方案，利用maven插件，在项目打包的时候将文件进行Hash，这样每次在发布版本的时候，只要文件本身发生了变化，文件的名称就会变化。

### 思路
整个实现的思路主要如下：
1. 复制webapp下的文件到临时文件夹；
2. 遍历临时文件夹里面的js和css文件，并进行Hash值计算，保存在Map中；
3. 遍历页面文件，替换里面的引用的js和css文件名。

> 插件主要的工作是完成2，3两步，第1步有插件已经实现了，拿来用就好。

### maven插件代码
``` java
    package com.coocaa.salad.maven.plugin;

    import com.origin.eurybia.utils.MD5Encode;
    import org.apache.maven.plugin.AbstractMojo;
    import org.apache.maven.plugin.MojoExecutionException;
    import org.apache.maven.plugins.annotations.LifecyclePhase;
    import org.apache.maven.plugins.annotations.Mojo;
    import org.apache.maven.plugins.annotations.Parameter;
    import org.codehaus.plexus.util.DirectoryScanner;

    import java.io.*;
    import java.util.Arrays;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;
    import java.util.stream.Collectors;

    @Mojo(name = "sourceHash", defaultPhase = LifecyclePhase.VALIDATE)
    public class ResourceHashMojo extends AbstractMojo {

        @Parameter(defaultValue = "${project.build.directory}/prepareWarSource")
        private File warSourceDirectory;

        @Parameter(defaultValue = "${project.build.directory}/prepareWarSource")
        private String warSourceDirectoryTrans;

        @Parameter(defaultValue = "${project.build.directory}/prepareWarSource")
        private File warSourceDirectoryReplace;
        @Parameter
        private String[] excludes;

        @Parameter
        private String[] includes;

        @Parameter
        private String[] includesHtml;

        @Parameter
        private String[] excludesHtml;

        @Parameter(defaultValue = "UTF-8")
        private String encode;

        @Parameter(defaultValue = "CRC32")
        private String algorithm;

        private Map<String, String> fileNameMap = new HashMap<>();

        public void execute() throws MojoExecutionException {
            List<File> files = getFiles(warSourceDirectory, getIncludes(), excludes);
            files.forEach(file -> {
                if (file.getParent().contains(warSourceDirectoryTrans)) {
                    String fileMd5 = MD5Encode.getFileMD5(file);
                    Integer index = file.getName().lastIndexOf(".");
                    String newFileName = file.getName().substring(0, index) + "_" + fileMd5 + file.getName().substring(index);
                    //给文件重命名
                    renameFile(file.getParent(), file.getName(), newFileName);
                    fileNameMap.put(file.getName(), newFileName);
                }
            });


            List<File> replaceFiles = getFiles(warSourceDirectoryReplace, getIncludesHtml(), excludesHtml);
            replaceFiles.forEach(file -> {
    //            System.out.println(file.getName() + "路径：" + file.getPath());
                replaceFileName(file);
            });

            //清除临时文件夹
            //deleteAllFilesOfDir(warSourceDirectory);
        }


        /**
         * 文件重命名
         *
         * @param path
         * @param oldName
         * @param newName
         */
        public void renameFile(String path, String oldName, String newName) {
            if (!oldName.equals(newName)) {//新的文件名和以前文件名不同时,才有必要进行重命名
                File oldFile = new File(path + "/" + oldName);
                File newFile = new File(path + "/" + newName);
                if (!oldFile.exists()) {
                    return;//重命名文件不存在
                }
                if (newFile.exists())//若在该目录下已经有一个文件和新文件名相同，则不允许重命名
                    System.out.println(newName + "已经存在！");
                else {
                    oldFile.renameTo(newFile);
                }
            } else {
                System.out.println("新文件名和旧文件名相同...");
            }
        }

        /**
         * 替换指定文件中的js，css文件名
         *
         * @param file
         */
        private void replaceFileName(File file) {
            String fileName = file.getName();
            String filePath = file.getParent();
            try {
                BufferedReader bufReader = new BufferedReader(new InputStreamReader(new FileInputStream(file)));//数据流读取文件
                StringBuffer strBuffer = new StringBuffer();
                final String[] temp = new String[1];
                while ((temp[0] = bufReader.readLine()) != null) {
                    //System.out.println(temp[0]);
                    //Matcher matcher = pattern.matcher(temp[0]);
                    if (temp[0].contains("<script ")) {
                        fileNameMap.forEach((key, value) -> {
                            if (temp[0].contains(key)) { //判断当前行是否存在想要替换掉的字符
                                temp[0] = temp[0].replace(key, value);//替换为你想要的东东
                            }
                        });
                        //System.out.println(temp[0]);
                    }
                    strBuffer.append(temp[0]);
                    strBuffer.append(System.getProperty("line.separator"));//行与行之间的分割
                }
                bufReader.close();

                PrintWriter printWriter = new PrintWriter(file.getParent() + "/temp.ftl");//替换后输出的文件位置
                printWriter.write(strBuffer.toString().toCharArray());
                printWriter.flush();
                printWriter.close();

                file.delete();

                renameFile(filePath, "temp.ftl", fileName);

            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        /**
         * 清空文件夹
         *
         * @param path
         */
        private void deleteAllFilesOfDir(File path) {
            if (!path.exists())
                return;
            if (path.isFile()) {
                path.delete();
                return;
            }
            File[] files = path.listFiles();
            for (int i = 0; i < files.length; i++) {
                deleteAllFilesOfDir(files[i]);
            }
            path.delete();
        }

        private List<File> getFiles(File parent, String[] in, String[] ex) {
            getLog().info("getFiles.....");
            DirectoryScanner scanner = new DirectoryScanner();
            scanner.setBasedir(parent);
            scanner.setIncludes(in);
            scanner.setExcludes(ex);
            scanner.scan();
            return Arrays.stream(scanner.getIncludedFiles()).parallel().map(fileName -> new File(parent, fileName)).collect(Collectors.toList());
        }


        private String[] getIncludes() {
            if (includes == null || includes.length < 1) {
                return new String[]{"**/*.js", "**/*.css"};
            }
            return includes;
        }

        private String[] getIncludesHtml() {
            getLog().info("getIncludesHtml");
            if (includesHtml == null || includesHtml.length < 1) {
                return new String[]{"**/*.jsp", "**/*.html", "**/*.ftl"};
            }
            return includesHtml;
        }


    }

 ```

#### pom.xml文件
``` xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <groupId>maven-plugin</groupId>
    <artifactId>salad-maven-plugin</artifactId>
    <modelVersion>4.0.0</modelVersion>
    <version>1.0-SNAPSHOT</version>
    <packaging>maven-plugin</packaging>
    <name>salad-maven-plugin</name>
    <url>http://maven.apache.org</url>
    <dependencies>
        <dependency>
            <groupId>org.apache.maven</groupId>
            <artifactId>maven-plugin-api</artifactId>
            <version>2.0</version>
        </dependency>
        <dependency>
            <groupId>org.apache.maven.plugin-tools</groupId>
            <artifactId>maven-plugin-annotations</artifactId>
            <version>3.2</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.1</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>com.origin.eurybia</groupId>
            <artifactId>origin-eruybia</artifactId>
            <version>3.9.4-RELEASE</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-plugin-plugin</artifactId>
                <version>3.3</version>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>

```

### 插件使用
1. 复制文件插件

``` xml
<plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.7</version>
                <executions>
                    <execution>
                        <phase>compile</phase>
                        <goals>
                            <goal>copy-resources</goal>
                        </goals>
                        <configuration>
                            <outputDirectory>${project.build.directory}/prepareWarSource</outputDirectory>
                            <resources>
                                <resource>
                                    <directory>${basedir}/src/main/webapp</directory>
                                </resource>
                            </resources>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
```

> 将webapp文件夹复制到prepareWarSource这个文件夹

2. 增加自己的插件
``` xml
<plugin>
                <groupId>maven-plugin</groupId><!--自己写的插件-->
                <artifactId>salad-maven-plugin</artifactId>
                <version>1.0-SNAPSHOT</version>
                <executions>
                    <execution>
                        <phase>compile</phase>
                        <goals>
                            <goal>sourceHash</goal>
                        </goals>
                        <configuration>
                            <warSourceDirectory>${project.build.directory}/prepareWarSource</warSourceDirectory>
                            <warSourceDirectoryTrans>${project.build.directory}/prepareWarSource/static/js/view</warSourceDirectoryTrans>
                            <warSourceDirectoryReplace>${project.build.directory}/prepareWarSource/WEB-INF/views</warSourceDirectoryReplace>
                            <includesHtml>
                                <include>**/*.jsp</include>
                                <include>**/*.html</include>
                                <include>**/*.ftl</include>
                            </includesHtml>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
```

3. 打war包
``` xml
<plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <configuration>
                    <warSourceDirectory>${project.build.directory}/prepareWarSource</warSourceDirectory>
                </configuration>
            </plugin>
```
