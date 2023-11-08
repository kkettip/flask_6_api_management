# flask_6_api_management


Steps to Create Cloud Functions in GCP

1. Navigate to Cloud Functions
  
2. Enable required APIs
   
3. Set configurations
   
4. Select Next
   
5. Select Deploy

6.Run the following in terminal to generate endpoint in browser
  
  `gcloud functions deploy function-1 \`
  
    --gen2 \
    
    --runtime=python311 \
    
    --region=us-east1 \
    
    --source=. \
    
    --entry-point=hello_http \
    
    --trigger-http \
    
    --allow-unauthenticated
  

Steps for API Documentation 

Modify code in app.py to the following:


`from flask import Flask, request, jsonify`

`from flasgger import Swagger`


`app = Flask(__name__)`

`Swagger(app)`


`@app.route('/hello', methods=['GET'])`

`def hello_get():`

    """
    
    This endpoint returns a greeting message.
    
    ---
    
    parameters:
    
      - name: name
      
        in: query
        
        type: string
        
        required: false
        
        default: World
        
    responses:
    
      200:
      
        description: A greeting message
        
    """
    
    name = request.args.get('name', 'World')
    
    return f'Hello {name}!'
    

`@app.route('/hello', methods=['POST'])`

`def hello_post():`

    """
    
    This endpoint returns a greeting message based on the name provided in the JSON body.
    
    ---
    
    parameters:
    
      - name: body
      
        in: body
        
        required: true
        
        schema:
        
          id: data
          
          required:
          
            - name
            
          properties:
          
            name:
            
              type: string
              
              default: World
              
    responses:
    
      200:
      
        description: A greeting message
        
      400:
      
        description: Invalid JSON
        
    """
    
    data = request.get_json()
    
    if data is None:
    
        return jsonify({'error': 'Invalid JSON'}), 400
        
    
    name = data.get('name', 'World')
    
    return jsonify({'message': f'Hello {name}!'})

`if __name__ == '__main__':`

    app.run(debug=True)


Deploy app in app engine to generate API documentation

Steps to deploy in GCP app engine

In terminal input :

1. `gcloud config set project <project ID>`
  
2. `gcloud auth login`

3. go to browser link

4. allow cloud SDK to access google account

5. copy authorization code 

7. enter authorization code

8. `gcloud app deploy app.yaml`

9. Web link will be provided `https://kettipcloud504.ue.r.appspot.com`

10. modify to become: `https://kettipcloud504.ue.r.appspot.com/apidocs/`

11. A swagger API documentation is displayed


Issues Encountered

Build Falied when trying to deploy cloud functions. This issue was resolved by adding  `functions-framework` in requirements.txt

