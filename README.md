RBAC using database
1. include the authManager in your configuration

 'authManager' => [
            'class' => 'yii\rbac\DbManager',
            'defaultRoles' => ['guest'],
        ],

2. create the tables by runnibg this on the command window, shell etc
   yii migrate --migrationPath=@yii/rbac/migrations

  create your roles, permissions or rule example bellow in a controller or command
  $auth = Yii::$app->authManager;
      	 		if($auth !=null){
      	    	// add "viewAboutPage" permission
      	    	$viewAboutPage = $auth->createPermission('viewAboutPage');
      	    	$viewAboutPage->description = 'User can view the About Page';
      	    	$auth->add($viewAboutPage);
      	 
      	    	// add "viewSignUpPage" permission
      	    	$viewSignUpPage = $auth->createPermission('viewSignUpPage');
      	    	$viewSignUpPage->description = 'User can view the sign up page';
      	    	$auth->add($viewSignUpPage);
      	 
      	    	// add "user" role and give this role the "viewAboutPage" permission
      	    	$users = $auth->createRole('users');
      	    	$auth->add($users);
      	    	$auth->addChild($users, $viewAboutPage);
      	 
      	    	// add "admin" role and give this role the "viewSignUpPage" permission
      	    	// as well as the permissions of the "usera" role
      	    	$admin = $auth->createRole('admin');
      	    	$auth->add($admin);
      	    	$auth->addChild($admin, $viewSignUpPage);
      	    	$auth->addChild($admin, $users);
      	 
      	    	// Assign roles to users. 1 and 2 are IDs returned by IdentityInterface::getId()
      	    	// usually implemented in your User model.
      	    	$auth->assign($users, 2);
      	    	$auth->assign($admin, 1);
      	 		}else{
      	 			echo "The roles already exists";
      	 		}
      	 		
      3.  use the roles to grant users access to specific actons
          that is it
