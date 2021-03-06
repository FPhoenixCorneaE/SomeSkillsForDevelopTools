


使用Android Studio将代码发布至Artifactory，具体步骤如下：

一、在项目下创建一个config文件夹，在文件夹下创建artifact_config.gradle文件，
	内容如下：
	
		/*
		* artifact_config.gradle文件配置主要包括几方面
		* 1. artifactory私有库地址,repoKey,账户信息;
		*/
		
		allprojects {
		
			ext {
				artifactory_address = ''
				artifactory_user = ''
				artifactory_password = ''
				artifactory_repoKey = ''
				artifactory_groupId = ''
			}
		
		}


​		
二、在项目的build.gradle文件中添加如下代码：
​	
	apply from: 'config/artifact_config.gradle'
	
	buildscript {
	
		repositories {
			jcenter()
		}
	
		dependencies {
			//Check for the latest version here: http://plugins.gradle.org/plugin/com.jfrog.artifactory
			classpath "org.jfrog.buildinfo:build-info-extractor-gradle:4.5.4"
		}
	}
	
	allprojects {
		repositories {
			jcenter()
		}
		apply plugin: "com.jfrog.artifactory"
		apply plugin: 'maven-publish'
	}


​	
三、在module的build.gradle文件中添加如下代码：

	// 发布渠道的AAR至artifactory仓库时，进行一下几步操作
	// 1.修改isRelease值为true
	// 2.修改publishVersion值，此值一般与渠道发布版本号一致
	// 3.执行artipublish任务，window系统下：gradlew &{moduleName}:artipublish、mac或linux系统下：./gradlew &{moduleName}:artipublish
	
	// 定义是否为发布状态
	def isRelease = true
	
	// 定义提交至Artifactory仓时AAR文件的版本名称，此值一般与渠道发布版本号一致
	def publishVersion = '1.2.5'
	
	// 定义library versionName值，按需定义
	def libsVersionName = '1.2.5'
	// 定义library versionCode值，按需定义
	def libsVersionCode = 125


​	
	android {
		
		defaultConfig {
			versionCode libsVersionCode
			versionName libsVersionName
		}
		
		// 项目配置结构
		sourceSets {
			main {
				if (isRelease) {
					assets.srcDirs = ['src/main/assets-release']
				} else {
					assets.srcDirs = ['src/main/assets']
				}
				res.srcDirs = ['src/main/res']
				java.srcDirs = ['src/main/java']
				aidl.srcDirs = ['src/main/aidl']
				jniLibs.srcDir 'src/main/jniLibs'
			}
		}
	
		// 这个是解决lint报错的代码
		lintOptions {
			quiet true
			abortOnError false
			ignoreWarnings true
			checkReleaseBuilds false// 方法过时警告的开关
			disable 'InvalidPackage' // Some libraries have issues with this.
			disable 'OldTargetApi' // Lint gives this warning but SDK 20 would be Android L Beta.
			disable 'IconDensities' // For testing purpose. This is safe to remove.
			disable 'IconMissingDensityFolder' // For testing purpose. This is safe to remove.
		}
	
		packagingOptions {
			exclude 'META-INF/DEPENDENCIES.txt'
			exclude 'META-INF/LICENSE.txt'
			exclude 'META-INF/NOTICE.txt'
			exclude 'META-INF/NOTICE'
			exclude 'META-INF/LICENSE'
			exclude 'META-INF/DEPENDENCIES'
			exclude 'META-INF/notice.txt'
			exclude 'META-INF/license.txt'
			exclude 'META-INF/dependencies.txt'
			exclude 'META-INF/LGPL2.1'
		}
	}
	
	dependencies {
		compile fileTree(dir: 'libs', include: ['*.jar'])
	}
	
	publishing {
		publications {
			aar(MavenPublication) {
				groupId = rootProject.ext.artifactory_groupId
				artifactId project.name
				version = publishVersion
				artifact "${project.buildDir}/outputs/aar/${project.name}-release.aar"
	
				//generate POM file
				pom.withXml {
					def root = asNode()
					def dependenciesNode = root.appendNode('dependencies')
					def repositoriesNode = root.appendNode('repositories')
					//Iterate over the compile dependencies (we don't want the test ones), adding a <dependency> node for each
					configurations.compile.allDependencies.each {
						if (it.group != null) {
							def dependencyNode = dependenciesNode.appendNode('dependency')
							dependencyNode.appendNode('groupId', it.group)
							dependencyNode.appendNode('artifactId', it.name)
							dependencyNode.appendNode('version', it.version)
						}
					}
					//add the repositories exclude local repositories,such as file:/D:/AndroidSDK/extras/android/m2repository/
					project.repositories.each {
						if (!it.url.toString().startsWith('file')) {
							def repositoryNode = repositoriesNode.appendNode('repository')
							repositoryNode.appendNode('url', it.url)
							repositoryNode.appendNode('name', it.name)
							repositoryNode.appendNode('releases').appendNode("enabled", true)
							repositoryNode.appendNode('snapshots').appendNode("enabled", false)
						}
					}
				}
			}
		}
	}
	
	artifactory {
		contextUrl = rootProject.ext.artifactory_address
		publish {
			repository {
				repoKey = rootProject.ext.artifactory_repoKey
				username = rootProject.ext.artifactory_user
				password = rootProject.ext.artifactory_password
			}
			defaults {
				publishArtifacts = true
				publications('aar')
				publishPom = true // Publish generated POM files to Artifactory (true by default)
				publishIvy = true
				// Publish generated Ivy descriptor files to Artifactory (true by default)
			}
		}
	}
	
	//在执行artifactoryPublish之前需要生成上传Maven仓库的Pom文档。
	artifactoryPublish {
	}.dependsOn(getTasks().getByPath(":" + project.name + ":generatePomFileForAarPublication"))
	
	if (isRelease) {
	
		task artipublish
	
		artipublish.dependsOn clean, assemble, artifactoryPublish
	
		assemble.mustRunAfter clean
		artifactoryPublish.mustRunAfter assemble
	
	}


​	
​	
​	