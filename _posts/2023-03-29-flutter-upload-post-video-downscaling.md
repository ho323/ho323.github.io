---
title: "[Flutter] 게시글 업로드 및 영상 다운 스케일링"
categories: Flutter
tags:
  - Flutter
  - Firebase
  - Application
  - Video
search: true
---

## 설계
기획했던 앱의 기본적인 틀은 만들었다.  
데이터베이스를 이용해서 게시글을 올리면 홈 피드에 표시될 수 있게 만들려고 한다.  

데이터베이스는 Firebase를 사용한다.  
HomeScreen에 FloatingActionButton 기능을 사용하여 언제든 버튼을 눌러서 게시글을 업로드 할 수 있게 설계했다.  
<img height="590" width="286" alt="1" src="https://user-images.githubusercontent.com/86637300/228469652-f0fb55f4-60e8-4d48-bd18-a66f57746878.png">  
홈 피드에서 공유된 게시글의 영상들과 댓글을 볼 수 있고, 좋아요를 누를 수 있다.  

<img height="590" width="286" alt="2" src="https://user-images.githubusercontent.com/86637300/228469650-729f001a-3083-449d-874c-44c91fe6fbfb.png">  
내가 혼자 디자인해서 좀 구리다.  

게시글에 운동 영상과 운동 일지를 기록할 수 있다.  
영상을 선택하고 저장을 누르면 현재 시간으로 기록되며 피드에 바로 공유가 된다.  

### 문제점
데이터베이스의 공간과 읽기쓰기는 용량이 제한되어 있다.  
나는 거지라서 이것을 해결하기 위해 영상을 업로드 할 때에 품질을 다운스케일링 할 필요가 있었다.  


## 영상 업로드
문제점을 해결하고 몇가지 기능을 사용하기 위해서 https://pub.dev/ 에서 플러그인을 찾았다.  

### Video Compress 
[video_compress | Flutter Package](https://pub.dev/packages/video_compress)  
Flutter 플러그인의 video_compress를 사용했다.  
```dart
import 'package:video_compress/video_compress.dart';

Future<void> _downscaleVideo(XFile vid) async {
  MediaInfo? mediaInfo = await VideoCompress.compressVideo(
    vid.path,
    quality: VideoQuality.LowQuality,
    deleteOrigin: true, // It's false by default
  );
}
```
문서를 참고해서 만들었다.  

### 파일 이름
파일 이름은 `업로드날짜-업로드시간-유저아이디.mp4` 이러한 형태로 설계했다.  
-를 기준으로 날짜, 시간, 유저아이디 정보를 가져올 수 있게 했다.  
```dart
void uploadVideo(XFile vid) async {
    File file = File(vid.path);
    DateTime now = DateTime.now();
    String fileName = '${now.year}_${now.month}_${now.day}_'
        '${now.hour}_${now.minute}_${now.second}_$userId.mp4';

    final storageRef = FirebaseStorage.instanceFor(
        bucket: "스토리지 주소").ref();
    final mountainsRef = storageRef.child(fileName);
    final mountainVideoRef = storageRef.child("video/$fileName");
    assert(mountainsRef.name == mountainVideoRef.name);
    assert(mountainsRef.fullPath != mountainVideoRef.fullPath);

    try {
      await mountainVideoRef.putFile(file);
    } on FirebaseException catch (e) {
      print(e);
    }
  }
```
이것 또한 Firebase 문서를 참고해서 만들었다.  

### 썸네일
<img height="590" width="286" alt="3" src="https://user-images.githubusercontent.com/86637300/228469639-b683ad86-2178-4919-95e4-43413cb68cf2.png">  
영상을 선택하면 +아이콘이 없어지고 영상의 썸네일이 등장한다.(영상의 첫 부분)  
썸네일을 클릭하면 영상을 다시 선택할 수 있다.  
```dart
Future<Uint8List?> _loadThumbnail(XFile vid) async {
  final thumbnail = await VideoCompress.getByteThumbnail(vid.path,
      quality: 50, // default(100)
      position: -1 // default(-1)
      );
  return thumbnail;
}
```
원시 데이터 형태로 만들게 했다.  
굳이 썸네일 사진을 저장할 필요가 없을거 같아서.  


<img width="766" alt="4" src="https://user-images.githubusercontent.com/86637300/228469632-fa0ac427-f97c-440f-801e-05bffea0885e.png">  
Firebase에서 확인해보면 잘 올라가 있다.  
