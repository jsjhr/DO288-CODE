Crear un nuevo proyecto liveness-readiness-healthchecks

	oc new-project probes

Crear un imagestream de la image de registry rhel7:7.3

	oc import-image rhel7 --from registry.lab.example.com:5000/rhel7:7.3 --confirm --insecure

Crear una nueva aplicación con la imagen rhel7:7.3

	oc new-app --image-stream=rhel7 --name probes

Agregar argumentos al deploymentconfig

	Editar el deploymentConfig:
	
	oc edit dc/checkprobe

    spec:
      containers:
      - args:
        - /bin/sh
        - -c
        - touch /tmp/healthy; touch /tmp/liveness; sleep 999999
        image: registry.lab.example.com:5000/rhel7:7.3
        imagePullPolicy: IfNotPresent
        name: checkprobe
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
```
spec:
  containers:
    -
    name: example-spring-boot
    image: 'image'
    command:
      - java
    args:
      - '-jar'
      - /opt/app-root/springboots2idemo.jar
```
Revisar que la aplicación esté ejecutando:

	oc get pods

Agregar los probe:

	oc set probe dc/checkprobe --readiness --initial-delay-seconds=5 --period-seconds=5 -- cat /tmp/healthy
	oc set probe dc/checkprobe --liveness --initial-delay-seconds=5 --period-seconds=5 -- cat /tmp/liveness

Hacer pruebas con el probe readiness y liveness:

En una terminal dejar el comando oc get pods -w ejecutando y abrir otra terminal para hacer las pruebas:

readiness
        oc rsh checkprobe-6-1lpv0   
        rm /tmp/healthy
El comando definido en el probe termina con un estado de error por lo que el pod pasa a estado not ready
        checkprobe-6-1lpv0   0/1       Running   0         1d
        touch /tmp/healthy

El comando definido en el probe termina con valor success por lo que el pod pasa a estado ready
checkprobe-6-1lpv0   1/1       Running   0         1d
LAB 2: Activating Probes


liveness
        rm -fr /tmp/livenes
        #El comando definido en el probe liveness termina el proceso principal del container y lo inicia nuevamente
command terminated with exit code 137

checkprobe-6-1lpv0   0/1       Error     0         1d
checkprobe-6-1lpv0   0/1       Running   1         1d
checkprobe-6-1lpv0   1/1       Running   1         1d


