# NETCO-RTSP
# NETCORTSP
# RTSPCAMERA
#  Vao nodemodules => rtsp-relay => index.js rồi chỉnh sửa đoạn này
 ffmpegPath,
      [
        ...(transport ? ['-rtsp_transport', transport] : []), // this must come before `-i [url]`, see #82
        '-i',
        url,
        '-f', // force format
        'mpegts',
        '-codec:v', // specify video codec (MPEG1 required for jsmpeg)
        'mpeg1video',
        '-r',
        '30', // 30 fps. any lower and the client can't decode it
        '-preset',
        'slow',
        '-crf',
        '18',
        '-b:v', '3M', 
        '-s',
         '1200x700',
        ...additionalFlags,
        '-',
      ],