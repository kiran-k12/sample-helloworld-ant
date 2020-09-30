def JAVA_CHANGED = true
def SPINNER_CHANGED = false
def INCLUSION_LIST = ""

pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
metadata:
  annotations:
    traffic.sidecar.istio.io/excludeOutboundIPRanges: "0.0.0.0/0"
  labels:
    some-label: some-label-value
spec:
  containers:
  - name: java-ant-container
    image: kanchari123/idq-java:1.0
    volumeMounts:
      - mountPath: /var/tmp
        name: test-volume
    command: 
    - cat
    tty: true
    resources:
      requests:
        memory: "3Gi"
        cpu: "3000m"
      limits:
        memory: "5Gi"
        cpu: "5000m"
  volumes:
  - name: test-volume
    hostPath:
      # directory location on host
      path: /var/tmp
"""
    }
  }
  stages {
    stage('Compile'){
        steps{
        container('java-ant-container'){
            script{
					if(JAVA_CHANGED){
						step ([$class: 'CopyArtifact',
							projectName: '2018x_OOTB_FD12',
							target: 'ootb']);
						withAnt(installation: 'Ant') {
							sh "ant clean retrieveOOTBSource compile dhDeployment"
						}
					}
				} // end script
        }
        }
    }
    
  }
}
