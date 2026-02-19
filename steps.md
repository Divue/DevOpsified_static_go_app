static go web app
this project doesnt have any devops practices

ci/cd, containerization, kubernaties cluster, 

1. containerizing -> multistage dockerfile, image with reduce size n security
2. kubernaties manifest deployement see 
3. CI - Github actions 
4. CD - using GITOPS ( argoCD)
5. Kubernaties cluster ( to delploy ci/cd, the app will be deployed on it eks cluster )
6. HELM chart ( in future dev team wants to deploy this app on dev, QA, Prod multiple environment, rather than writting manifest for each environment they can use the helm chart and pass the values on yaml)
7. Ingress Controller -> to create a load balancer depending on ingress config so that the app is exposed to outside world , also how ot map the load balancer ip add to the local dns so that we test app is accessable form outside world

THESE ARE THE THINGS IMPLEMENTING IN THIS PROJECT



getting started with it
STEP 1: Containerization of this project
	first understand the project, how app is build, run on which port its running and how it looks like
	first run the app locally, therefore clone it first, then build the app using the json file, execute a go binary ( app is running ), now write multistage DOCKERFILE, stage1 baseimae, stage 2 always mentiona we used a distroless image,
	
    1.first build the app
      go build -o main(random name) . // builds tha app
    2.now execute the go binary thats been created
      ./main  //runs the app on localhost:8080/courses 
    3.write multistage dockerfile 
    4.docker build -t divue/go-web-app:v1 . //build dockerfile
    5. docker run -p 8080:8080 -it divue/go-web-app:v1 // runs the container
    6. push image to registry to use it in k83
    (kubernaties cluster will pull image from image registry/local)

STEP2: Kubernaties manisfest
    1. create a deployemet,service, ingress file in k8s
    2. validate the kubernaties manifest that we hv written, for that we'll need to use kubernaties cluster and today we'll use aws eks

STEP3: eks
    1. install kubectl, eksctl, awsCLI
    2. install eks cluster
        eksctl create cluster --name demo-cluster --region ap-south-1 
    3. deployment 
        kubectl apply -f k8s/manifests/deployment.yaml (pod app running)
    4. verify if my service manifest is implemented correctly 
        kubectl apply -f k8s/manifests/service.yaml
    5. same for ingress kubectl apply -f k8s/manifests/ingress.yaml
    at this point we cant get access the resouce directly from the ingress we'll need a ingress controller for that 

    before we move with  ingress controller config lets veify if the service is working properly
    kubectl edit svc go-web-app it didnt worked earlier coz the inbound trafic wasnot activated

Step4: ingress controller config
    kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.14.3/deploy/static/provider/aws/deploy.yaml

    3 things to remember for ingress 
    ingress, ingress controller, load balancer
     ingress controller watches the ingress resources and creates load balancer 

    DNS mapping take the load balancer (kubectl get ing) then run nslookup <addof_load_Balancer> to get the ip address then go to sudo vim /etc/hosts then go at last add the ip add u got form the nslookup with the go-web-app.local that u set in ingress.yaml
    now t app will work on go-web-app.local/courses
    in orgnization we'll use the company host name

STEP5: Helm chart creation
