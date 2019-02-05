# RequetDataSet
The following data set is generated based on collected data in :

Craig Gutterman, Katherine Guo, Sarthak Arora, Xiaoyang Wang, Les Wu, Ethan Katz-Bassett, Gil Zussman, “Requet: Real-Time QoE Detection for Encrypted YouTube Traffic,” in Proc. ACM MMSys’19 (to appear), 2019.

We would appreciate it if you cite this paper when publishing results that use the provided data.

Please contact Craig Gutterman (clg2168@columbia.edu) with any questions.

The dataset is divided into 5 group folders for data from groups A, B1, B2, C, D, along with a summary file named 'ExperimentInfo.txt' for the entire dataset. Each line in the file describes an experiment using the following four attributes: (a) experiment number, (b) video ID, (c) initial video resolution, and (d) length of experiment in seconds. 

A group folder is further divided into two subfolders, one for PCAP files, and the other for txt files. Each experiment is described by a PCAP file and a txt file. The PCAP file with name in the form of (i) 'baseline_{date}_exp_{num}.pcap'$ is for an experiment where the end device is static for the entire duration whereas a file with name in the form of (ii) 'movement_{date}_exp_{num}.pcap' is for an experiment where the end device movement occurs during the experiment. The txt file names end with 'merged.txt'. The txt file contains data colletect from YouTube API and summary of PCAP trace for the experiment.

In each $'merged.txt'$ file, data is recorded for each 100$ ms interval. Each interval is represented as: [Relative Time, # Packets Sent, # Packets Received, # Bytes Sent, # Bytes Received, [Network Info 1], [Network Info 2], [Network Info 3], [Network Info 4], [Network Info 5], ... , [Network Info 25], [Playback Info] ]. 

Relative Time marks the end of the interval. Relative Time is defined as the time since the JAvascript Node server hosting the YouTube API is started. Relative Time for the 0th interval is defined as 0 sec. It is updated in intervals of 100 ms. TShark is called prior the Javascript Node server. Therefore, the 0th interval contains Wireshark data up to the start of the Javascript Node server.

The Network Info i is represented as: [IP Src, IP Dst, Protocol, # Packets Sent, # Packets Received, # Bytes Sent, # Bytes Received] for each interval. IP_Src is the IP address of the end device. The top 25 destination IP addresses in terms of total bytes sent and received for the entire session are recorded. For each $i$ of the top 25 IP_Dst addresses, the Protocol associated with the higher data volume for the interval (in terms of total number of packets exchanged) is selected, and data volume in terms of packets and bytes for each interval is reported for the {IP_Src, IP_Dst, Protocol} tuple in [Network Info i].

The Playback Info is represented as: [Playback Event, Epoch Time, Start Time, Playback Progress, Video Length, Playback Quality, Buffer Health, Buffer Progress, Buffer Valid]. From the perspective of video playback, a YouTube session can contain three exclusive regions: buffering, playing, and paused. YouTube IFrame API considers a transition from one playback region into another as an event. It also considers as an event any call to the API to collect data. The API enables the recording of an event and of detailed information about playback progress at the time the event occurs. Epoch Time marks the epoch time of the most recent YouTube API data collected in that interval. Playback Info records events occurred during the 100-ms interval.

Each field of Playback Info is defined as follows:

Playback Event - This field is a binary array with four indexes for the following states: 'buffering', 'paused', 'playing', and 'collect data'.  The 'collect data' event occurs every $100$ ms once the video starts playing. For example, an interval with a Playback Event [1,0,0,1] indicates that playback region has transitioned into 'buffering' during the 100-ms interval and a 'collect data' event occurred.

Epoch Time - This field is the UNIX epoch time in milliseconds of the most recent YouTube API event in the 100-ms interval. 

Start Time - This field is the Unix epoch time in milliseconds of the beginning of the experiment.

Playback Progress - This field reports the number of seconds the playback is at epoch time from the start of the video playback. 

The Video Length - This field reports the length of the entire video asset (in seconds).  

Playback Quality - This field is a binary array of size 9 with indices for the following states: unlabelled, tiny (144p), small (240p), medium (360p), large (480p), hd720, hd1080, hd1440, and hd2160. The unlabeled state occurs when the video is starting up, buffering, or paused. For example, a Playback Quality [0, 1, 1, 0, 0, 0, 0, 0, 0] indicates that during the current interval, video playback experienced two quality levels - tiny and small.

Buffer Health - This field is defined the amount of buffer in seconds ahead of current video playback. It is calculated as: 

Buffer Health = Buffer Progress * Video Length - Playback Progress

Buffer progress - This field reports the fraction of video asset that has been downloaded into the buffer. 

Buffer valid - This field has two possible values: True or -1. True represents when data is being collected from the YouTube IFrame API. -1 indicates when data is not being collected from the YouTube IFrame API during the current interval. 

