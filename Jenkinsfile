#!/usr/bin/groovy
 @Library('github.com/fabric8io/fabric8-pipeline-library@master')
 def dummy
 dockerTemplate {
   mavenNode {
    ws{
      checkout scm
      sh "git remote set-url origin git@github.com:fabric8io/kubeflix.git"

      def pipeline = load 'release.groovy'

      stage 'Updating dependencies'
      def prId = pipeline.updateDependencies('http://central.maven.org/maven2/')

      stage 'Stage'
      def stagedProject = pipeline.stage()

      stage 'Approve'
      pipeline.approveRelease(stagedProject)

      stage 'Promote'
      pipeline.release(stagedProject)
      if (prId != null){
        pipeline.mergePullRequest(prId)
      }

      stage 'Update downstream dependencies'
      pipeline.updateDownstreamDependencies(stagedProject)
    }
  }
}
