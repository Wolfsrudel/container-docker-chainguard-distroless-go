# go

<!---
Note: Do NOT edit directly, this file was generated using https://github.com/chainguard-images/readme-generator
-->

[![CI status](https://github.com/chainguard-images/go/actions/workflows/release.yaml/badge.svg)](https://github.com/chainguard-images/go/actions/workflows/release.yaml)

Container image for building Go applications.

## Get It!

The image is available on `cgr.dev`:

```
docker pull cgr.dev/chainguard/go:latest
```

## Supported tags

| Tag | Digest | Arch |
| --- | ------ | ---- |
| `1` `1.19.2` `1.19.2-r0` `latest` | `sha256:d5de47388e7a516a219a140daa3119179ae7b5bd8f1867859a02219cad7b2c4b`<br/>[View entry in Rekor](https://rekor.tlog.dev/?hash=sha256:d5de47388e7a516a219a140daa3119179ae7b5bd8f1867859a02219cad7b2c4b) | `386` `amd64` `arm64` `armv6` `armv7` `ppc64le` `riscv64` `s390x` |
| `1.19` | `sha256:95d128b0ff9c62258d98c4c90894f1bf9bc6f47781629a656635374b15476736`<br/>[View entry in Rekor](https://rekor.tlog.dev/?hash=sha256:95d128b0ff9c62258d98c4c90894f1bf9bc6f47781629a656635374b15476736) | `386` `amd64` `arm64` `armv6` `armv7` `ppc64le` `riscv64` `s390x` |
| `1.19.1-r0` | `sha256:38adede21243fda456da1a69eaaa0e2291c145dd9ab648332a5a932c0bc61490`<br/>[View entry in Rekor](https://rekor.tlog.dev/?hash=sha256:38adede21243fda456da1a69eaaa0e2291c145dd9ab648332a5a932c0bc61490) | `386` `amd64` `arm64` `armv6` `armv7` `ppc64le` `riscv64` `s390x` |
| `1.19.1` `1.19.1-r2` | `sha256:ec841359408345c28596a5765253da3870c8364d010cfd55455f6f31e129faf2`<br/>[View entry in Rekor](https://rekor.tlog.dev/?hash=sha256:ec841359408345c28596a5765253da3870c8364d010cfd55455f6f31e129faf2) | `386` `amd64` `arm64` `armv6` `armv7` `ppc64le` `riscv64` `s390x` |
| `1.19.1-r0-glibc` | `sha256:a6e1ba3be1063a0adef4c4640bcd20484197b8b84bd79d114e518f0e59e15d9b`<br/>[View entry in Rekor](https://rekor.tlog.dev/?hash=sha256:a6e1ba3be1063a0adef4c4640bcd20484197b8b84bd79d114e518f0e59e15d9b) | `amd64` |
| `1.19.1-r1` | `sha256:fc6136ed867b98663f6cd5388805eae81bd21de20944b4fe4a4b6748673d36cf`<br/>[View entry in Rekor](https://rekor.tlog.dev/?hash=sha256:fc6136ed867b98663f6cd5388805eae81bd21de20944b4fe4a4b6748673d36cf) | `386` `amd64` `arm64` `armv6` `armv7` `ppc64le` `riscv64` `s390x` |


## Usage

## Host architecture example

To build the Go application in [examples/hello/main.go](examples/hello/main.go)
using the host architecture:

```
docker run --rm -v "${PWD}:/work" -w /work/examples/hello \
    -e GOOS="$(go env GOOS)" -e GOARCH="$(go env GOARCH)" \
    cgr.dev/chainguard/go build -o /work/hello .
```

The example application will be built to `./hello`:
```
$ ./hello
Hello World!
```

## Dockerfile example

The following example Dockerfile builds a hello-world program in Go and copies it on top of the `cgr.dev/chainguard/static:latest` base image:

```dockerfile
# syntax=docker/dockerfile:1.4
FROM cgr.dev/chainguard/go:latest as build

WORKDIR /work

COPY <<EOF go.mod
module hello
go 1.19
EOF

COPY <<EOF main.go
package main
import "fmt"
func main() {
    fmt.Println("Hello World!")
}
EOF
RUN go build -o hello .

FROM cgr.dev/chainguard/static:latest

COPY --from=build /work/hello /hello
CMD ["/hello"]
```

Run the following command to build the demo image and tag it as `go-hello-world`:

```shell
docker build -t go-hello-world  .
```

Now you can run the image with:

```shell
docker run go-hello-world
```

You should get output like this:

```
Hello World!
```

It’s worth noting how small the resulting image is:

```shell
docker images go-hello-world
```

```
REPOSITORY       TAG       IMAGE ID       CREATED       SIZE
go-hello-world   latest    859fedabd532   5 hours ago   3.21MB
```

## Signing

All Chainguard Images are signed using [Sigstore](https://sigstore.dev)!

<details>
<br/>
To verify the image, download <a href="https://github.com/sigstore/cosign">cosign</a> and run:

```
COSIGN_EXPERIMENTAL=1 cosign verify cgr.dev/chainguard/go:latest | jq
```

Output:
```
Verification for cgr.dev/chainguard/go:latest --
The following checks were performed on each of these signatures:
  - The cosign claims were validated
  - Existence of the claims in the transparency log was verified offline
  - Any certificates were verified against the Fulcio roots.
[
  {
    "critical": {
      "identity": {
        "docker-reference": "ghcr.io/chainguard-images/go"
      },
      "image": {
        "docker-manifest-digest": "sha256:d5de47388e7a516a219a140daa3119179ae7b5bd8f1867859a02219cad7b2c4b"
      },
      "type": "cosign container image signature"
    },
    "optional": {
      "1.3.6.1.4.1.57264.1.1": "https://token.actions.githubusercontent.com",
      "1.3.6.1.4.1.57264.1.2": "schedule",
      "1.3.6.1.4.1.57264.1.3": "1792c2bd8b465c08248ce7b5a5cb59f9b684be4f",
      "1.3.6.1.4.1.57264.1.4": "Create Release",
      "1.3.6.1.4.1.57264.1.5": "chainguard-images/go",
      "1.3.6.1.4.1.57264.1.6": "refs/heads/main",
      "Bundle": {
        "SignedEntryTimestamp": "MEQCIDT55PP98C5zutY+K998oYzMVZcm1lbE9Fqz/yg1/foKAiAWMj7gOIioLe9OXCYKoZfe4Zg9FnLcFlXkPe91Qy8vmw==",
        "Payload": {
          "body": "eyJhcGlWZXJzaW9uIjoiMC4wLjEiLCJraW5kIjoiaGFzaGVkcmVrb3JkIiwic3BlYyI6eyJkYXRhIjp7Imhhc2giOnsiYWxnb3JpdGhtIjoic2hhMjU2IiwidmFsdWUiOiJlNjNjODg5MmVjYjFkNTc3YmFlNTRiNzM2MDIzOGMzMmU4NzhhMDU4YzgwNDA1YTkxNWZkM2U3ZmRiOWExYTI2In19LCJzaWduYXR1cmUiOnsiY29udGVudCI6Ik1FVUNJUURvQXlndWRWTXRkcGEwS1pza2tub0JzN1pUZ0pZeTZ1M1Y1VnBHSEhiZ1NBSWdQR3ZKNmEzOEFYcTBYRGl5bythRmtXWk00UXpQNTZTaEgxNGpiRy8wSUVVPSIsInB1YmxpY0tleSI6eyJjb250ZW50IjoiTFMwdExTMUNSVWRKVGlCRFJWSlVTVVpKUTBGVVJTMHRMUzB0Q2sxSlNVUndSRU5EUVhsdFowRjNTVUpCWjBsVlUzaEpOazlsVTBaRlEybEtaUzh5Wms1dmJtUjBUSHA0YURoQmQwTm5XVWxMYjFwSmVtb3dSVUYzVFhjS1RucEZWazFDVFVkQk1WVkZRMmhOVFdNeWJHNWpNMUoyWTIxVmRWcEhWakpOVWpSM1NFRlpSRlpSVVVSRmVGWjZZVmRrZW1SSE9YbGFVekZ3WW01U2JBcGpiVEZzV2tkc2FHUkhWWGRJYUdOT1RXcEplRTFFU1ROTlJFbDRUVlJWZDFkb1kwNU5ha2w0VFVSSk0wMUVTWGxOVkZWM1YycEJRVTFHYTNkRmQxbElDa3R2V2tsNmFqQkRRVkZaU1V0dldrbDZhakJFUVZGalJGRm5RVVZCZVd0aFl6Tm5SM0pSV0RabkwzbFBURll6ZERSVmQwMUhRblp1Y0N0WkwxaDZPWGNLWW5VelZ6YzNSakZWTVZsWWMxZG9hblZpVVVWQ1JGaG9hRFl2YldkM1FqWkhaa1ZHU205clJWRkVSbEJzTm14dVRVdFBRMEZyWjNkblowcEZUVUUwUndwQk1WVmtSSGRGUWk5M1VVVkJkMGxJWjBSQlZFSm5UbFpJVTFWRlJFUkJTMEpuWjNKQ1owVkdRbEZqUkVGNlFXUkNaMDVXU0ZFMFJVWm5VVlZKV21KT0NsVjVOREo1UkdOd2VsSXhhelJTZUU1YU5qWlpSeTl6ZDBoM1dVUldVakJxUWtKbmQwWnZRVlV6T1ZCd2VqRlphMFZhWWpWeFRtcHdTMFpYYVhocE5Ga0tXa1E0ZDFwQldVUldVakJTUVZGSUwwSkdiM2RYU1ZwWFlVaFNNR05JVFRaTWVUbHVZVmhTYjJSWFNYVlpNamwwVERKT2IxbFhiSFZhTTFab1kyMVJkQXBoVnpGb1dqSldla3d5WkhaTWVUVnVZVmhTYjJSWFNYWmtNamw1WVRKYWMySXpaSHBNTTBwc1lrZFdhR015VlhWbFYwWjBZa1ZDZVZwWFducE1NbWhzQ2xsWFVucE1NakZvWVZjMGQwOVJXVXRMZDFsQ1FrRkhSSFo2UVVKQlVWRnlZVWhTTUdOSVRUWk1lVGt3WWpKMGJHSnBOV2haTTFKd1lqSTFla3h0WkhBS1pFZG9NVmx1Vm5wYVdFcHFZakkxTUZwWE5UQk1iVTUyWWxSQlYwSm5iM0pDWjBWRlFWbFBMMDFCUlVOQ1FXaDZXVEpvYkZwSVZuTmFWRUV5UW1kdmNncENaMFZGUVZsUEwwMUJSVVJDUTJkNFRucHJlVmw2U21sYVJHaHBUa1JaTVZsNlFUUk5hbEUwV1RKVk0xbHFWbWhPVjA1cFRsUnNiVTlYU1RKUFJGSnBDbHBVVW0xTlFuZEhRMmx6UjBGUlVVSm5OemgzUVZGUlJVUnJUbmxhVjBZd1dsTkNVMXBYZUd4WldFNXNUVU5KUjBOcGMwZEJVVkZDWnpjNGQwRlJWVVVLUmtkT2IxbFhiSFZhTTFab1kyMVJkR0ZYTVdoYU1sWjZUREprZGsxQ01FZERhWE5IUVZGUlFtYzNPSGRCVVZsRlJETktiRnB1VFhaaFIxWm9Xa2hOZGdwaVYwWndZbXBEUW1sbldVdExkMWxDUWtGSVYyVlJTVVZCWjFJNFFraHZRV1ZCUWpKQlFXaG5hM1pCYjFWMk9XOVNaRWhTWVhsbFJXNUZWbTVIUzNkWENsQmpUVFF3YlROdGRrTkpSMDV0T1hsQlFVRkNhRUpqTVhoTE9FRkJRVkZFUVVWamQxSlJTV2RVVVcwM01WcDViR2xSTDJwSWFtUmlVbHBQYlZaRWRrTUtjREpPT1hOTE5tUkxiR013ZUZZME5IQjRRVU5KVVVSc1ZXWnVNM0ZpWjJoYWJXb3ZhM1oyVlZOR1NHazBRemt2UmxSdFlUbEZjemR5VUd0NVJXbHJUUXBaYWtGTFFtZG5jV2hyYWs5UVVWRkVRWGRPY0VGRVFtMUJha1ZCTldWUkwySnBjM2RTTVZKdVFuYzFhMUZHTTJwdmQzaFJWa3BEVW5CWVlXd3JWMnhJQ21sTlJ6ZGtLMGwyUkRoRFIxVndTR1JYUms1NWRXZHNTalZSVkVKQmFrVkJkblZQYUVoMmRYQkNTV0ZhVFVGc2JUaEJlbTVOVWtsWkwxYzVhVmMzVkNzS1NFTTJaMjltU21kR1V6TklRbGhKYWk5UVEyazBZWHBUVEVVMWNIVmthMm9LTFMwdExTMUZUa1FnUTBWU1ZFbEdTVU5CVkVVdExTMHRMUW89In19fX0=",
          "integratedTime": 1666836729,
          "logIndex": 5948803,
          "logID": "c0d23d6ad406973f9559f3ba2d1ca01f84147d8ffc5b8445c224f98b9591801d"
        }
      },
      "Issuer": "https://token.actions.githubusercontent.com",
      "Subject": "https://github.com/chainguard-images/go/.github/workflows/release.yaml@refs/heads/main",
      "githubWorkflowName": "Create Release",
      "githubWorkflowRef": "refs/heads/main",
      "githubWorkflowRepository": "chainguard-images/go",
      "githubWorkflowSha": "1792c2bd8b465c08248ce7b5a5cb59f9b684be4f",
      "githubWorkflowTrigger": "schedule",
      "run_attempt": "1",
      "run_id": "3333963480",
      "sha": "1792c2bd8b465c08248ce7b5a5cb59f9b684be4f"
    }
  }
]
```

You can verify that the image was built in Github Actions in this repository from the `Issuer` and `Subject` fields.
</details>

## Build

This image is built with [apko](https://github.com/chainguard-dev/apko).

