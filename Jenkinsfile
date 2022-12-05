final String semVerRegEx = /^v?(0|[1-9]\d*)\.(0|[1-9]\d*)\.(0|[1-9]\d*)(?:-((?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*)(?:\.(?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*))*))?(?:\+([0-9a-zA-Z-]+(?:\.[0-9a-zA-Z-]+)*))?$/

boolean commitAuthorIsFidelityAutomator = false

pipeline {
    agent { node { label 'swarm' } }
    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        disableConcurrentBuilds()
        timeout(time: 60, unit: 'SECONDS')
    }
    stages {
        stage("Process 'type' of commit author") {
            steps {
                script {
                    withCredentials([gitUsernamePassword(credentialsId: 'Fidelity package builder', gitToolName: 'Default')]) {
                        def author = sh(returnStdout: true, script: "git log -1 --pretty=format:'%an'").trim()
                        echo "$author"
                        if (env.GIT_AUTHOR_NAME == author) {
                            commitAuthorIsFidelityAutomator = true
                        }
                    }
                }
            }
        }
        stage('UPM publish') {
            // 'tag should be semver' could be validated earlier, i.e. before local creation and push to remote.
            when {
                allOf {
                    tag comparator: 'REGEXP', pattern: semVerRegEx
                    expression { !commitAuthorIsFidelityAutomator }
                }
            }
            steps {
                script {
                    // for tag discovered Jenkins builds neither env GIT_BRANCH nor BRANCH_NAME do provide the branch that was tagged and instead have the tag.
                    // This is why the code below is getting the branch name.
                    // Furthermore by using the code solution below, we know if the tag was on top of the branch or not,
                    // and we do not want to proceed with upm publish if tag not on top of branch,
                    // as downstream jobs commits and pushes package.json (and Changelog) and thus needs to be on top of branch.

                    String ret = sh returnStdout: true, script: "git show -s --pretty=%d HEAD"
                    // Example of return string when tag is on top of a branch
                    // (HEAD, tag: testtag1, origin/tags/testtag1, origin/git-tag-trigger-publish-experiment)
                    // (HEAD, tag: v1.1.2-preview.gittag.24, tag: 1.1.2-preview.gittag.25, tag: origin/tags/v1.1.2-preview.gittag.24, origin/tags/1.1.2-preview.gittag.25, origin/git-tag-trigger-publish-experiment)
                    // Example of return string when tag is NOT on top of branch
                    // (HEAD, tag: 1.1.2-preview.gittag.23, origin/tags/1.1.2-preview.gittag.23)

                    String branch = ''
                    def splitFirst = ret.split(',');
                    for (item in splitFirst) {
                        if (!item.contains('tag: ') && !item.contains('/tags/') && !item.contains('HEAD')) {
                            branch = item.split('/')[1] - ')'
                            break
                        }
                    }
                    if (branch == '') {
                        error("The tag ${TAG_NAME} is not on top of a branch, as 'git show' and 'git for-each-ref' shows. Update the tag to the top of the branch to make a upm publish.")
                    }
                    build job: 'resolutiongames/upm-publish/main', parameters: [
                        string(name: 'repoUrl', value: GIT_URL),
                        string(name: 'branch', value: branch),
                        string(name: 'version', value: TAG_NAME),
                        booleanParam(name: 'only validate', value: false),
                        string(name: 'upstreamWorkspacePath', value: WORKSPACE)
                    ]
                }
            }
        }
    }
    post {
        unsuccessful {
            script {
                String msg = "${TAG_NAME} release failed, <${BUILD_URL}|Build Url>. Contact the Fidelity team for further support."
                slackSend channel: '#fidelity-releases-verbose', message: msg, color: 'danger'
            }
        }
    }
}
