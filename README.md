
## Set F — Booking State and Cancellation Consistency

### Build and Test
```bash
mvn clean test
mvn clean package
```

### Docker
```bash
docker build -t campus-equipment-booking:set-f .
docker run -d --name campus-booking -p 8080:8080 campus-equipment-booking:set-f
```

Health check: `http://localhost:8080/actuator/health`

### Kubernetes
```bash
minikube start --driver=docker
minikube image load campus-equipment-booking:set-f
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl get deployments
kubectl get pods
kubectl get services
```

### Health Check Through Kubernetes
```bash
kubectl port-forward service/campus-booking-service 8090:8080
```

Open `http://localhost:8090/actuator/health`.

### Scaling
```bash
kubectl scale deployment campus-booking --replicas=2
kubectl get deployments
kubectl get pods
```

### Set F Change Request
- Reject booking quantities that are non-positive or exceed available inventory.
- Allow cancellation only for confirmed bookings.
- Restore inventory exactly once when a booking is cancelled.
- Test capacity boundaries and repeated cancellation.
