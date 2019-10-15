## In controller application_controller.erb 

	class ApplicationController < ActionController::Base
		def after_sign_in_path_for(resource)
			stored_location_for(resource) || your_root_path
	  end
	end

## In routes.rb

	namespace :controller_redirection do
	  root :to => "controller#method"
	end


## Ex of redirection

==> redirect if current_user.is_admin? in case of a Dashboard Admin

	class ApplicationController < ActionController::Base
		def after_sign_in_path_for(resource)
			if current_user.is_admin?
				stored_location_for(resource) || admin_root_path
	    end
	  end
	end

==> redirect in case of a Dashboard User

	class ApplicationController < ActionController::Base
		def after_sign_in_path_for(resource)
				stored_location_for(resource) || user_path(current_user)
	    end
	  end
	end


==> if Dashboard user and admin

	class ApplicationController < ActionController::Base
	def after_sign_in_path_for(resource)
		if current_user.is_admin?
			stored_location_for(resource) || admin_root_path
		else
      stored_location_for(resource) || user_path(current_user)
    end
	  end
	end