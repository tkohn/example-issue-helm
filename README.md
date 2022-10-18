# Example Issue Helm

This project is a minimal Helm chart example to debug a Helm issue with mutating webhooks.

The mutating webhook will add a initContainer and volumeMount to the deployment with an initContainer.
This has to be done manually. 

## Run the example

Clone the project and install the chart via:

```
helm upgrade --install example-issue-helm .
```

Verify that the pod is in running state.

Now edit the Deployment via: `kubectl edit deployment example-issue-helm`
Add the following snippet

```
        volumeMounts:
        - name: workdir
          mountPath: /usr/share/nginx/html
      initContainers:
      - name: install
        image: busybox:1.28
        command:
        - wget
        - "-O"
        - "/work-dir/index.html"
        - http://info.cern.ch
        volumeMounts:
        - name: workdir
          mountPath: "/work-dir"
      volumes:
      - name: workdir
        emptyDir: {}
```

after the following lines

```
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
```

and save the deployment.

Run the helm upgrade command to trigger a patch with `helm upgrade --install --set testLabel=test example-issue-helm .`
This command should work and you will not see any errors.