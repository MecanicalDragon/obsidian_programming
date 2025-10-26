To arrange video streaming using **AWS Stack** you have to use the following resources:

| Resource Name                      | Resource Type                     | Where to find |
| ---------------------------------- | --------------------------------- | ------------- |
| MediaLiveChannel                   | AWS::MediaLive::Channel           | MediaLive     |
| MediaLiveInput                     | AWS::MediaLive::Input             | MediaLive     |
| MediaLiveEndRollFileInput          | AWS::MediaLive::Input             | MediaLive     |
| MediaPackageChannel                | AWS::MediaPackage::Channel        | MediaPackage  |
| MediaPackageHlsLiveEndpoint        | AWS::MediaPackage::OriginEndpoint | MediaPackage  |
| MediaPackageHlsHarvestEndpoint     | AWS::MediaPackage::OriginEndpoint | MediaPackage  |
| MediaPackageCloudFrontDistribution | WS::CloudFront::Distribution      | CloudFront    |
Every resource can be reviewed in **CloudFormation** on the *Resources* tab. Every specific resource can be found only in its dedicated zone; the zone can be selected in the top-right corner of the AWS UI. If we've got a video *pipeline* with id `ee-31`...
### MediaLive

**MediaLiveChannel** is used to accept video streams. Its name shown in **AWS Elemental MediaLive** is the corresponding *pipeline* id. It has 2 media inputs: *Live* and *LiveEndRollFile*.

**MediaLiveInput** of `RTMP_PUSH` type (push type) has the same name as a channel (*pipeline* id) and provides 2 rtmp-endpoints (for more reliable streaming) to push video stream to:
 ```
"rtmps": [
    "rtmp://10.8.200.55:1935/app-ee-31/pkey-ee-31",
    "rtmp://10.8.203.59:1935/app-ee-31/skey-ee-31"
]
```
This input accepts video stream and saves it to S3 bucket. `app-` and `pkey-` URL parameters refer to the corresponding *pipeline* id and **MediaLiveChannel** and **MediaLiveInput** names.

**MediaLiveEndRollFileInput** of type `MP4_FILE` (pull type) has a name that consists of the *pipeline* id with an `-end-roll-file-mp4` suffix. It is designed to provide MP4 files saved by the first input in the S3 storage to the **MediaPackageChannel**.

[AWS Input types](https://docs.aws.amazon.com/medialive/latest/ug/inputs-supported-formats.html)
### MediaPackage

**MediaPackageChannel** has a physical id identical to the *pipeline* id and 2 endpoints: *harvest* and *live*.

**MediaPackageHlsLiveEndpoint** is used to fetch HLS video for the live-streaming.

**MediaPackageHlsHarvestEndpoint** is used to fetch HLS video for the recording purposes.
### CloudFront

**MediaPackageCloudFrontDistribution** is a [[CDN]] cell instance. Its instances deliver video stream to the clients and host *EDGE Lambdas* (Lambdas that intentionally reside closer to the clients to reduce network delay). Some EDGE lambdas hosted by the distribution can be:
- `cdn-authorization` to authorize a client to permit video receiving
- `live-playlist-rewrite` to modify iPhone client requests to fetch the playlist (`m3u8`) data due to the lameness of Apple products.
```
"cdns": [
    "https://xxx.cloudfront.net/out/v1/32...76/index.m3u8"
]
```

`http://xxx.cloudfront.net/` is the domain name of the CDN cell; the rest of the URI - path to the playlist file (`m3u8`).

*Distribution* logs can be found in the *Logs* side tab. But if the distribution is deleted already, we can go to the pipeline instance, and in `settings -> edit` find the S3 bucket with logs where we can download logs in `gz` file format.

*EDGE Lambda* logs can be viewed in **CloudWatch** in *Logs Groups*, where they can be filtered by the AWS Region hosting functions, lambda name, and streaming time. The *log group* name is formatted as `/aws/lambda/<aws-region>.<function-name>`.
