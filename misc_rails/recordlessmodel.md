Creating a form without a database backing it

- rails g model string string id text 
- rails g controller new create

- in view <%= link_to "Submit form query", new_particles_path(:particle_id => @particle) =%> or submit form button with the form action form_for @paritcle

- particles controller

        def new
          #particle = particle.new(particle_id => params
        new

        def create
          @particle = Particle.new(params[:particle])
          if @particle.save
            flash[:notice] = "Successfully saved particle"
            redirect_to root_url
          else
            render :action => 'new'
          end
        end


        class Recommendation < ActiveRecord::Base
          def self.columns() @columns ||= []; end
 
          def self.column(name, sql_type = nil, default = nil, null = true)
            columns << ActiveRecord::ConnectionAdapters::Column.new(name.to_s, default, sql_type.to_s, null)
          end
  
          column :from_email, :string
          column :to_email, :string
          column :article_id, :integer
          column :message, :text
  
          validates_format_of :from_email, :to_email, :with => /^[-a-z0-9_+\.]+\@([-a-z0-9]+\.)+[a-z0-9]{2,4}$/i
          validates_length_of :message, :maximum => 500
  
          belongs_to :article
        end



        def new
          @recommendation = Recommendation.new(:article_id => params[:article_id])
        end

        def create
          @recommendation = Recommendation.new(params[:recommendation])
          if @recommendation.valid?
            # send email
            flash[:notice] = "Successfully created recommendation."
            redirect_to root_url
          else
            render :action => 'new'
          end
        end

- When generating the model the migration needs to be rolled back, but the request will cause an error
- To override the database storage just override the methods in the model 

        class Recommendation < ActiveRecord::Base
          class_inheritable_accessor :columns
          self.columns = []

          def self.column(name, sql_type = nil, default = nil, null = true)
            columns << ActiveRecord::ConnectionAdapters::Column.new(name.to_s, default, sql_type.to_s, null)
          end

          column :recommendable_type, :string
          column :recommendable_id, :integer
          column :email, :string
          column :body, :text

          belongs_to :recommendable, :polymorphic => true

          validates_presence_of :recommendable
          validates_associated :recommendable
          validates_format_of :email, :with => /^$|^\S+\@(\[?)[a-zA-Z0-9\-\.]+\.([a-zA-Z]{2,4}|[0-9]{1,4})(\]?)$/ix
          validates_presence_of :body
        end

- Routing - the form submit posts the form information to the controller, the controller asks the model for the data, the model queries the API, sets the instance variable and returns the data to the view
- Can also turn the external API calls into an instantiated class in Rails
- Query information comes from the form as params(:key), query API in controller using GET!, pass data to model, passed to model as a Model.new, Model.etc call (with optional params arguments assigned), the method is then called and calls the HTTP post request to the API with the proper query string constructed (move into helper method)