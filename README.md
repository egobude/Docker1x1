# Docker 1x1

## Usage
    
### Clone the repository

    $ git clone https://github.com/egobude/Docker1x1.git 
   
### Start the container
   
    $ docker run -d -p 82:80 -v $(pwd):/usr/share/nginx/html nginx:1.13-alpine
    
And go to [http://0.0.0.0:82](http://0.0.0.0:82)