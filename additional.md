1) kubectl set image deployment/nginx-deploy nginx=nginx:1.17 --record
kubectl annotate deployment nginx-deploy --overwrite message="Updated nginx image to 1.17" //set annotation to the deployemnt
