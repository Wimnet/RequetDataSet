# RequetDataSet
The following data set is generated based on collected data in :

Craig Gutterman, Katherine Guo, Sarthak Arora, Xiaoyang Wang, Les Wu, Ethan Katz-Bassett, Gil Zussman, “Requet: Real-Time QoE Detection for Encrypted YouTube Traffic,” in Proc. ACM MMSys’19 (to appear), 2019.

We would appreciate it if you cite this paper when publishing results that use the provided data.

Please contact Craig Gutterman (clg2168@columbia.edu) with any questions.

The dataset has 5 folders for the data from groups A, B1, B2, C, D. In addition, it contains a file 'EpxerimentInfo.txt'. Each line in the file contains an experiment number, video ID, initial video resolution, and length of experiment in seconds. For a given group, there are two subfolders  with the  following files for each experiment in the group:  (i) PCAP files and (ii)  files with  data collected from the YouTube API as well as summarized data from the PCAP traces. The former file name is structured as $baseline_{date}_exp_{num}.pcap' for static experiments where the end device is in a fixed location for the entire experiment and 'movement_{date}_exp_\{num}.pcap' for experiments where the end device is moved during the experiment. The latter associated file names end with '_merged.txt' instead of '.pcap'. The rest of the document describes experiment data that end with 'merged.txt'. 

In each $'merged.txt'$ file, data is recorded approximately every $100$ ms. Each interval is represented as: [Relative Time, # Packets Sent, # Packets Received, # Bytes Sent, # Bytes Received, [Network Info 1], [Network Info 2], [Network Info 3], [Network Info 4], [Network Info 5], ... , [Network Info 10], [Playback Info] ]. The Relative Time is the time of the first packet recorded from the PCAP file for the experiment. The Relative Time starts from 0 and data is summarized every $100$ ms. The Network Info is represented as: [IP Src, IP Dst, Protocol, \# Packets Sent, # Packets Received, # Bytes Sent, # Bytes Received] for the 100 ms interval. The IP Src is the IP address of the client device. The top 10 destination IP addresses for the entire session are chosen. The top is determined by the total amount of packets. For each of these there is Network Info for the interval.  The associated protocol for a pair in an interval is determined by the most common protocol associated to the IP pair. 

The Playback Info is represented as: [Playback Event, Epoch Time, Start Time, Playback Progress, Video Length, Playback Quality, Buffer Health, Buffer Progress, Buffer Valid]. Each one of these fields is represented as follows:

Playback Event - The YouTube IFrame API environment allows to record any event as it occurs and to keep detailed information about playback progress. The YouTube IFrame API gives changes to 3 states: ''Buffering', 'Pause', and 'Play'.  We additionally collect data every 100 ms to record updates to buffer level and video quality. Accordingly, the Playback Event field is a binary array with indexes for the following states: 'Buffering', 'Pause', 'Play', and 'Collect Data'. The 0th index represents  the Buffering event indicating the player has switched to the stall state and the video is buffering. The 1st index represents a Pause event by the user. The 2nd index represents Play event of the video starting or switching from buffering state. The 3rd index represents a Collect Data event in which data is collected without change in YouTube player state. This event occurs every $100$ ms without changing in 'Buffering', 'Pause', or 'Play'.  For example, an interval with a Playback Event such as [1,0,0,1] indicates that the video switched to the Buffering State and a Collect Data event in the same interval.

Epoch Time - The Epoch time of the current sample from YouTube IFrame API (in milliseconds). 

Start Time - The Epoch time of the beginning of the experiment (in milliseconds). 

Playback Progress - The number of seconds the playback is from the start of the video. 

The Video Length - The length of the video (in seconds). 

Playback Quality - Is a binary array with indexes for the following states: unlabeled, tiny, small, medium, large, hd720, hd1080, hd1440, hd2160. Playback Quality is represented with a list of size 9, respectively. The unlabeled state can occur when the video is starting up, video is buffering, or paused. For example, an interval with a Playback Quality such as [0, 1, 1, 0, 0, 0, 0, 0, 0] indicates that there were tiny and small states recorded in the same interval.

Buffer Health - The buffer health is defined the amount of buffer ahead of buffer playback (in seconds). The buffer health is calculated as: 

Buffer Health = Buffer Progress * Video Length - Playback Progress

Buffer progress - The fraction of video loaded in the buffer. 

Buffer valid - Is either True or $-1$. True represents when data is being collected from the YouTube IFrame API. $-1$ is given when data is not being collected from the YouTube IFrame API. 

