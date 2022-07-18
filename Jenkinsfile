pipeline {
    agent any

    stages {
        stage('Preparing gradlew') {
            steps {
                sh 'chmod +x gradlew'
            }
        }
        stage('test') {
            steps {
                sh './gradlew test'
            }
        }
        stage('build') {
            steps {
                sh './gradlew build'
            }
        }
        stage('Release') {
            steps {
                sh 'token="Token"'
                sh 'tag=$(git describe --tags)'
                sh 'message="$(git for-each-ref refs/tags/$tag --format=\'%(contents)\')"'
                sh 'name=$(echo "$message" | head -n1)'
                sh 'description=$(echo "$message" | tail -n +3)'
                sh 'release=$(curl -XPOST -H "Authorization:token $token" --data \'{"tag_name": "$tag", "target_commitish": "main", "name": "$name", "body": "$description", "draft": false, "prerelease": false}\' "https://api.github.com/repos/MadumathiS/caesar-cipher/releases")'
            }
        }
        stage('Artifacts-Upload') {
            steps {
                sh 'tag=$(git describe --tags)'
                sh 'upload=$(curl -f -XPOST -H "Authorization: $token" -H "Content-Type:application/octet-stream" --data-binary @build/libs/caesar-cipher.jar "https://uploads.github.com/repos/MadumathiS/caesar-cipher/releases/$tag/assets?name=caesar-cipher.jar")'
                
            }
        }
        stage('Deploy') {
            steps {
                sh 'java -jar ./build/libs/caesar-cipher.jar '
            }
        }
    }
}