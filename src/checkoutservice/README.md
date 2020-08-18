# checkoutservice

Run the following command to restore dependencies to `vendor/` directory:

    dep ensure --vendor-only

## Local Development

Set Default GO:

```bash
gvm use go1.14 --default
```

Install Go Packages used by application (resolves IDE issues - single example below):

```bash
$ go get contrib.go.opencensus.io/exporter/jaeger
```

Build local application:

```bash
$ go build -gcflags='-N -l' -o ./checkoutservice .
```

```bash
$ ls -l ./checkoutservice
-rwxr-xr-x  1 giranm  staff  24444300 16 Aug 23:54 ./checkoutservice
```

Local Run:

```bash
$ eval `cat .env` ./checkoutservice
{"message":"Tracing enabled.","severity":"info","timestamp":"2020-08-17T00:01:03.060848+01:00"}
{"message":"Profiling enabled.","severity":"info","timestamp":"2020-08-17T00:01:03.061017+01:00"}
{"message":"jaeger initialization disabled.","severity":"info","timestamp":"2020-08-17T00:01:03.06106+01:00"}
{"message":"service config: \u0026{productCatalogSvcAddr:productcatalogservice:3550 cartSvcAddr:cartservice:7070 currencySvcAddr:currencyservice:7000 shippingSvcAddr:shippingservice:50051 emailSvcAddr:emailservice:5000 paymentSvcAddr:paymentservice:50051}","severity":"info","timestamp":"2020-08-17T00:01:03.061076+01:00"}
{"message":"Stats enabled.","severity":"info","timestamp":"2020-08-17T00:01:03.061511+01:00"}
{"message":"starting to listen on tcp: \"[::]:5050\"","severity":"info","timestamp":"2020-08-17T00:01:03.06156+01:00"}
{"message":"failed to start profiler: project ID must be specified in the configuration if running outside of GCP","severity":"warning","timestamp":"2020-08-17T00:01:03.12647+01:00"}
{"message":"sleeping 10s to retry initializing Stackdriver profiler","severity":"info","timestamp":"2020-08-17T00:01:03.12656+01:00"}
{"message":"failed to initialize stackdriver exporter: stackdriver: google: could not find default credentials. See https://developers.google.com/accounts/docs/application-default-credentials for more information.","severity":"info","timestamp":"2020-08-17T00:01:03.126509+01:00"}
{"message":"sleeping 10s to retry initializing Stackdriver exporter","severity":"info","timestamp":"2020-08-17T00:01:03.126586+01:00"}
```

Access gRPC UI Helper:

```bash
$ grpcui -plaintext -proto ../../pb/demo.proto localhost:5050
```

## Docker Development

Docker Build & Run:

```bash
$ docker build -t giran/checkoutservice:0.0.2-test .
```

```bash
$ docker push giran/checkoutservice:0.0.2-test
```

```bash
$ docker run -d -p 5050:5050 --name checkout --env-file .env giran/checkoutservice:0.0.2-test
```
