# Jetson_ffmpeg_trancode_cluster

 This project aims to bring together a few things together. 

The inciting goal was to:
lower my power consumptions
learn aarch64 (AKA 64 bit arm)
learn about clustering 
implement platform agnostic hardware accelerated transcoding for ffmpeg

## Components 

### Hardware:

 1. Host server (either dedicated or worskstation PC)
 2. Networking switch (poe?)
 3. Multiple Nvidia Jetson's (ideally NX (agx is the dream)) but probably 2gb nano's or, depending on launch dates of Nvidia roadmap, Jetson Nano Next or Jetson Orin 
 
 ### Software: 
 1. Host 
  a. OS for server- debian based 
  b. Unicorn Transcoder or Kuber Plex 
  c. NFS share (Data itself is accessed over the network on my nas, ZFS and so on)
  d. Custom capture scrip to modify arguments sent from PMS to transcoder 
  e. Plex Media Server
  f. Load balancer  
 
 2. Jetson (aka transcoder node)
  a. Jetpack (4.3?)
  b. client side of UT or KP from 1.b
  c. custom ffmpeg build
   i. Personal cuda patch (should upstream to newest branch once I'm done) for tonemapping. currently reihard, but will change to eotf 2390 eventually
   ii. Jcover90 ffmpeg patch to enable the use of the transcode blocks 
   iii. nvidia build of ffmpeg to enable decoding and vf_scale_cuda
  d. client side of load balancer from 1.f
  e. (things I forgot will go here)
  
## How do I plan to architect this?

As of now (dec 2020) the intention is to handle the different aspects using these projects:

The distribution of work load via the Unicorn transcoder project [link goes here]

Transcoding of content handled via ffmpeg [link to website here]

Jetson NANO does not currently have a great FFMPEG implementation for what I need. need to patch in 

As of now the most optimal way to achieve what we want is to decode in hardware, use cuda to resize and tonemap if needed, then encode the file while sending back to the host.      
    
## Current Work in progress

cuda based tonemapping filter: changed /cuda_filter/vf_scale_cuda as a workaround to perform tonemapping using reinhard 

see ffmpeg-devel mailing list for more. 
