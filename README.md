<a href="https://colab.research.google.com/github/MagMithu17/Google-Colab-remove-unwanted-audio-tracks-subtitles-from-video-with-folder-batch-scan-feature-/blob/main/Clone_Media_with_selective_Stream_Track(Video%2CAudio%2CSubtitles).ipynb?authuser=1">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a><br>

# Google-Colab-Remove-Unwanted-Video-Audios,Subtitles-Using-Cloning-Method
### Step 1 - Notedown required stream numbers from cell 1 (media info cell)
### Step 2
   ![Alt Text](https://i.ibb.co/3mdYMJh/clone-media-with-selective-track.png)

```
#@title <font color ="yellow">`⬅`</font><font color="#01c968">`Clone Media with selective Stream Track(Audio,Subtitles)`</font>
from IPython.display import clear_output
from pathlib import Path
import time ,os

def get_file_size(file_path):
    import os
    size = os.path.getsize(file_path)
    return size
def convert_bytes(size, unit=None):
    if unit == "KB":
        return 'File size: ' + str(round(size / 1024, 3)) + ' Kb'
    elif unit == "MB":
        return str(round(size / (1024 * 1024), 3)) + ' Mb'
    elif unit == "GB":
        return 'File size: ' + str(round(size / (1024 * 1024 * 1024), 3)) + ' Gb'
    else:
        return 'File size: ' + str(size) + ' bytes'
SourName = []
SourSize = []
DestName = []
DestSize = []

Delete_Source_Media_File = False #@param {type:"boolean"}
while True:
  Media_File_Path = "" #@param {type:"string"}
  Save_Folder_Path = "/content/sample_data" #@param {type:"string"}
  if not Save_Folder_Path.strip():
    Save_Folder_Path = "/content"
  from pathlib import Path
  Path(Save_Folder_Path).mkdir(parents=True, exist_ok=True)
  Enter_Audio_Subtitle_Stream_Num = "" #@param {type:"string"}
  Output_Media_Name = "" #@param {type:"string"}
  Output_Media_Name = Output_Media_Name.strip()
  import string
  args = ""
  m = "-map 0:"
  def remove(string_input):
    return "".join(string_input.split())
  def contains_only_digits(s):
    for ch in s: 
      if not ch in string.digits: 
        return False
    return True
  if not Enter_Audio_Subtitle_Stream_Num.strip():
    print("Error : Stream_Num Field is empty")
    break 
  if(str(contains_only_digits(remove(Enter_Audio_Subtitle_Stream_Num)))=="False"):
    print("Enter Valid Stream Numbers   Eg : 1 3 4 6")
    break
  s = [d for d in Enter_Audio_Subtitle_Stream_Num if d.isdigit()] 
  for x in s: 
    v = str(x)
    q = m+v+"? "
    args = args+q
  map = str(args).rstrip()
  import os
  if os.path.isfile(Media_File_Path):
    if not Output_Media_Name.strip():        #chk Output_Media_Name Field empty or not
      from pathlib import Path    
      Output_Media_Name = Path(Media_File_Path).name
      DestName.append("\t"+str(Path(Media_File_Path).name))  #add Distination file name to DistName array
    else:
      DestName.append("\t"+str(Output_Media_Name))                #add Distination file name to DistName array
    from pathlib import Path
    
    SourName.append(str(Path(Media_File_Path).name))  #add source file name to SourName array

    S_size = get_file_size(Media_File_Path)           
    SourSize.append("--> "+str(convert_bytes(S_size, "MB"))) #add Source file size to SourSize Array

    myTuple = (Save_Folder_Path, Output_Media_Name)
    DestPath = "/".join(myTuple)
    !ffmpeg -v error -hide_banner -i "$Media_File_Path" $map -c copy "$DestPath"
    
    D_size = get_file_size(DestPath)
    DestSize.append("--> "+str(convert_bytes(D_size, "MB"))) #add destination file size to DestSize array
    time.sleep(2)
    print(DestPath+" Created")
    if Delete_Source_Media_File:
      !rm "$Media_File_Path"
      time.sleep(2)
      print(Media_File_Path+" is deleted")
    clear_output()
    break     
  else:
    import os
    for root, dirs, files in os.walk(Media_File_Path):
      for file in files:
          if file.endswith(".mp4") or file.endswith('.mkv') or file.endswith('.wmv') or file.endswith('.ts') or file.endswith('.flv'):
               from pathlib import Path
               Media_File_Path = os.path.join(root, file)
               FileName = Path(os.path.join(root, file)).name
               myTuple = (Save_Folder_Path, FileName)
               DestPath = "/".join(myTuple)
               
               SourName.append(str(Path(Media_File_Path).name))  #add source file name to SourName array

               S_size = get_file_size(Media_File_Path)
               SourSize.append("--> "+str(convert_bytes(S_size, "MB")))  #add Source file size to SourSize Array

               !ffmpeg -v error -hide_banner -i "$Media_File_Path" $map -c copy "$DestPath"
               
               DestName.append("\t"+str(Path(DestPath).name))                #add Distination file name to DistName array 
               
               D_size = get_file_size(DestPath)    
               DestSize.append("--> "+str(convert_bytes(D_size, "MB")))  #add destination file size to DestSize array

               clear_output()
               print(DestPath+" Created")
               time.sleep(2)
               if Delete_Source_Media_File:
                 !rm "$Media_File_Path"
                 print(Media_File_Path+" is deleted")
                 time.sleep(2)
               clear_output()                             
    break
res = "\n".join("{} {} {} {}".format(x, y, z, v) for x, y, z, v in zip(SourName, SourSize, DestName, DestSize))
print(res)                                              
```
