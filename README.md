### Private PyPi Server on Kubernetes

1) Create .htpasswd password file using the following command  
`htpasswd -sc .htpasswd <username>`
2) Create kubernetes secret for the above password file  
`kubectl create secret generic pypi-auth-secret --from-file=.htpasswd`
3) Deploy pypi-server  `kubectl apply -f deployment.yaml`
5) For uploading a package on private pypi, use the following command after running `python setup.py sdist` in package directory (requires username and password created in step 1),  
`twine upload --repository-url https://private-pypi.cf dist/*` 
5) For downloading,  
    a)  Create a directory `.pip` in USER's home directory and create a file named `pip.conf` with the following content   
    ```
    [global]
    extra-index-url = https://<username>:<password>@private-pypi.cf
    trusted-host = private-pypi.cf
    ```
    b)  Install package using pip `pip install <package-name>`
