pipeline {
    agent {
        node {
            label 'android-boot-image-builder'
        }
    }

    stages {
        stage('Download kernel') {
            steps {
                    sh '''
                        bash /opt/common/scripts/1_fetch_kernel.sh lineage-17.1
                    '''
                }
        }

        stage('Build kernel') {
            steps {
                    sh '''
                        bash /opt/common/scripts/2_build_kernel.sh 
                    '''
                }
        }

        stage('Repackage kernel') {
            steps {
                    sh '''
                        bash /opt/common/scripts/3_copy_kernel.sh && \
                        bash /opt/common/scripts/4_fix_permissions.sh
                    '''
                }
        }

        stage('Copy boot image to master node') {
            steps {
                sh 'cp /out/boot.img "${WORKSPACE}/gemini_lineage_17.1_boot.img"'
            }
        }

        stage('Publish boot image on S3') {
            steps {
               archiveArtifacts artifacts: 'gemini_lineage_17.1_boot.img', onlyIfSuccessful: true
            }
        }
  }
}
