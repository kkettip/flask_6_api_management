# flask_6_api_management


Steps to Create Cloud Functions in GCP

1. Navigate to Cloud Functions
  
2. Enable required APIs
   
3. Set configurations
   
4. Select Next
   
5. Select Deploy

6.Run the following in terminal to generate endpoint in browser
  
  gcloud functions deploy function-1 \
  
    --gen2 \
    
    --runtime=python311 \
    
    --region=us-east1 \
    
    --source=. \
    
    --entry-point=hello_http \
    
    --trigger-http \
    
    --allow-unauthenticated
  

Steps for API Documentation 

Modify code in appy.py to the following:

from flask import Flask, request, jsonify

from flasgger import Swagger


app = Flask(__name__)

Swagger(app)


@app.route('/hello', methods=['GET'])

def hello_get():

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
    

@app.route('/hello', methods=['POST'])

def hello_post():

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

if __name__ == '__main__':

    app.run(debug=True)
