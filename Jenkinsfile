properties([
    [$class: 'RebuildSettings', autoRebuild: false, rebuildDisabled: false], [$class: 'JobRestrictionProperty'], 
    parameters([
        choice(name: 'environment_name', choices: ['lab','development', 'production'], description: 'Specify the environment'),
    ])
])

node("plcislave04"){

	println("env.environment_name = " + env.environment_name)
	println("env.BRANCH_NAME = ${env.BRANCH_NAME}")
	println("NODE_NAME = ${env.NODE_NAME}")
	println("env.GIT_BUILD_CAUSE = " + env.GIT_BUILD_CAUSE )
	
	sh 'mkdir -p .git && chmod -R u+w .git'
    unstash "workspace"
    sh 'chmod -R u+rwx .git'

	// on_commit
	on_commit to: /^(.*)[Ff]eature(.*)/, {
		println(">>>> on_commit feature, so lint...")
		linting()
	}

	// on_pull_request
	on_pull_request to: /^(.*)[Dd]ev(elop|elopment|eloper|)$/,{
		println(">>>> on_pull_request to dev, so lint...")
		linting()
	}	

	on_pull_request to: /^(.*)[Ss]table/,{
		println(">>>> on_pull_request to stable, so lint...")
		linting()
		// TODO: some level of integration required
	}	

	on_pull_request to: /^(.*)[Rr]elease$/,{
		println(">>>> on_pull_request to release, so lint...")
		linting()
	}	
	
	// on_pull_request to: /^([Mm]ain|[Mm]aster)$/,{
	// 	println(">>>> on_pull_request to main || master, so lint...")
	// 	linting()
	// }


	// on_merge events
	on_merge to: /^(.*)[Dd]ev(elop|elopment|eloper|)$/,{				
		linting()					
		sh 'cat ./molecule/default/verify.yml'
		molecule_test()
	}

	on_merge to: /^(.*)[Ss]table/,{	
		linting()

		dir("merged"){
			molecule_test()	
		}		
	}			

	on_merge to: /^(.*)[Rr]elease$/,{
		linting()

		dir("merged"){
			molecule_test()	
		}		
	}	

	on_merge to: /^([Mm]ain|[Mm]aster)$/,{
		linting()

		dir("merged"){
			molecule_test()	
		}		
	}	
}
