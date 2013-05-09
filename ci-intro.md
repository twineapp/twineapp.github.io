Instructions for CI API Maintenance
===================================


Intro
-----

- The REST controller we are using for this API is the CodeIgniter Rest Server. Their readme provides a quick overview and is available at https://github.com/philsturgeon/codeigniter-restserver
- Syntax we follow for server code: http://ellislab.com/codeigniter/user-guide/general/styleguide.html with the exception we don't use OR's and 
AND's
- Our workflow process for Git: http://webhis.github.io/siv/
- For MySQL queries, we use the CodeIgniter's Active Record Class to allow information to be retrieved, inserted, and updated in the database with minimal scripting. Tutorial and code is available at: http://ellislab.com/codeigniter/user-guide/database/active_record.html
- For MongoDB queries, we use the CodeIgniter MongoDB Active Record Library. Tutorial and code is available at: https://github.com/alexbilbie/codeigniter-mongodb-library


How to Add a New Feature
------------------------

- Create an entry in the api/application/config/routes.php (ie: $route['some_feature/some_action'] = 'some_feature/some_action';)
- Create a controller in api/application/controllers for the new feature (ie: some_feature.php) and open other controllers and reuse basic structure up to and including the _contruct. Change the class name and comments as necessary, and at the end of the _contruct add your model (ie: $this->load->model('some_feature');). Remove the loading of any models that you will not be using. For each action that you add to your controller, you will need to have 4 functions (ie: some_action_get, some_action_post, some_action_put, some_action_delete) to handle the different HTTP requests using our RESTful API. Each function requires a call to 'check_action_permissions' to check if the user is allowed to perform the selected action and permissions are stored in api/application/config/siv.php. Most successful responses from the API should be JSON encoded and can be sent via: $this->response(array('success' => TRUE, 'data' => $data_arr), 200);
- Create a model in api/application/models for the new feature (ie: some_feature_model.php) and open other models and reuse basic structure up to and including the _contruct. Add the various functions you need here and include comments for each function. Remember we want to keep the controllers thin and the models fat.
- To make an API request via get, goto the URL: http://localhost/siv-v3/api/index.php/some_feature/some_action


Creating or Adding a Library
----------------------------

- Libraries that are specific to the this project should be created in api/application/libraries, prefixed with a SIV_ and loaded via: $this->load->library('SIV_Dropbox');
- Third party libraries should be added in api/application/third_party and loaded via: require($this->siv->paths['ci'] . 'third_party' . DS . 'some_library_folder' . DS . 'some_library.php');


