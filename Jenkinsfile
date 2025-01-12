pipeline {
    agent {
        label 'seh_01'
    }
    environment {
		//This can be nexus3 or nexus2
		NEXUS_VERSION = "nexus3"
		// This can be HTTP or HTTPS
		NEXUS_PROTOCOL = "http"
		// Where your nexus is running
		NEXUS_URL = "192.168.18.11:8081"
		// Repository where we will upload the artifact
		NEXUS_REPOSITORY = "mvn"
		// Jenkins credential id to authenticate to Nexus OSS
		NEXUS_CREDENTIAL_ID = "nexusCredential"
}
    stages {
        stage("git checkout") {
            steps { 
                git 'https://github.com/sumitgit01/LoginWebApp_mysql.git'
            }
        }
        stage("build") {
            steps {
                sh """
                    echo '**Building the project**'
                    pwd
                    ls -alrt
                    maven clean package
                    echo '**Build completed**'
                """
            }
        }
    }
stage ("deploy to nexus") {
    steps {
        script {
            pom = readMavenPom file: "pom.xml";
            filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
            echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
            artifactPath = filesByGlob[0].path;
            artifactExists = fileExists artifactPath;
            if(artifactExists) {
                echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                nexusArtifactUploader(
                    nexusVersion: NEXUS_VERSION,
                    protocol: NEXUS_PROTOCOL,
                    nexusUrl: NEXUS_URL,
                    groupId: pom.groupId,
                    version: ARTIFACT_VERSION,
                    repository: NEXUS_REPOSITORY,
                    credentialsId: NEXUS_CREDENTIAL_ID,
                    artifacts: [
                        [artifactId: pom.artifactId,
                        classifier: '',
                        file: artifactPath,
                        type: pom.packaging]
                    ]
                );
            } else {
                        error "*** File: ${artifactPath}, could not be found";
            }
        }
    }
}