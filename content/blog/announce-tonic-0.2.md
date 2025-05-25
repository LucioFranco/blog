+++
title = "Tonic: 0.2 with the future in mind!"
slug = "tonic-0-2-release"
date = 2020-04-01T10:00:00-04:00
+++

# Tonic: 0.2 release with the future in mind!

After a few months of work we finally have released `tonic` 0.2. This
release marks the start of a new path for `tonic`.

## Generic encoding

Originally, `tonic` was a wrapper around `hyper`, `tower` and `prost` providing a
good out of the box experience for protobuf based gRPC services. With 0.2 `tonic`
_still_ provides a great out of box experience for protobuf but now also
supports other encoding formats! `tonic` has always supported custom codecs
via the `tonic::codec::Codec` trait but codegen would always tie directly to
`prost`. Which means that if you wanted to write a custom codec you would have
to manually replicate what `tonic-build` does. This is no longer true as we now
have traits that can be implemented within `tonic-build` to guide codegen in the
correct direction.

In the future, `tonic` will also provide support for flatbuffers via the new
work in-progress [`butte`] crate! `tonic` 0.2 supports the groundwork for this
and we will create an accompanying crate that will provide all the types you need
in the future.

[`butte`]: https://github.com/butte-rs/butte

## Healthchecks

Another common feature request has been providing a way to support gRPC
healthchecks. After some work [@jen20] has implemented the `tonic-health`
crate which provides support for server side health reporting.

```rust
let (mut health_reporter, health_service) = tonic_health::server::health_reporter();

// Set our Greeter service to serving.
health_reporter
        .set_serving::<GreeterServer<MyGreeter>>()
        .await;

// Add healthcheck service to our router
Server::builder()
    .add_service(health_service)
    .add_service(GreeterServer::new(greeter))
    .serve(addr)
    .await?;
```

Example can be found [here].

[@jen20]: https://github.com/jen20
[here]: https://github.com/hyperium/tonic/blob/master/examples/src/health/server.rs

## v0.2.0

Thanks to all the wonderful contributors for helping get this release over the finish
line. As always, there is a  [changelog] and
an [issue] tracker if you run into any issues. Please, feel welcome to also join us on [discord]
if you need any help!

[changelog]: https://github.com/hyperium/tonic/blob/master/CHANGELOG.md
[issue]: https://github.com/hyperium/tonic/issues
[discord]: https://discord.gg/tokio
