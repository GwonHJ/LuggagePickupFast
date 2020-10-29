# LPF(Luggage Pickup Fast) / RFID를 이용한 수화물도착 알림 서비스

# 경북대학교 SW융합 해커톤 출품작

프로젝트명 : LPF (Luggage Pickup Fast)

팀명 : VIST

팀원 : 권현지, 석수민, 송민혜, 유소정, 장하림

시연영상 : https://youtu.be/vpl9_RSFvDs

개발 플랫폼 : 안드로이드 스튜디오, Firebase, Google Spreadsheet

Android App개발 담당

(개발 당시 Github사용방법을 몰라서 다른 팀원이 전체 코드를 업로드하였습니다.)

## 개발개요
    
    RFID를 이용하여 수하물 예상 도착 알림 서비스 어플리케이션
    
    개발 필요성 : 수하물용 비행기가 연착이 되었을때 무한정 기다리게 되면서 생각해낸 아이디어
    
    타켓 : 연착된 비행기를 기다리는 사람
    
    
## 상세내용
    
    항공사에서 RFID를 등록한 후 어플리케이션 사용자에게 태그를 주면 사용자는 그 태그를 이용하여 어플리케이션에 자동으로 로그인한다.
    
    이때 항공사에서 등록되지 않은 태그는 로그인이 되지 않는다(NFC가 있는 카드들이 아무거나 읽는것을 방지하기 위해서)
    
    수하물이 비행기에서 내리면서 태그가 리더기에 읽히면 사용자는 수하물이 도착한 정보를 휴대폰으로 알 수 있다.
    
## 주요 기능

- RFID tag를 이용한 로그인
 - 데이터 서버에 저장
 - RFID 리더기에 tag가 읽히면 사용자에게 알림
 - 수화물 도착 예정 시간 표시
 - 수령에 문제 발생시 상담원 연결


## 실행화면 예시
![LPf](https://user-images.githubusercontent.com/45057466/97435875-6cefa200-1964-11eb-83cf-8f7fef290ba9.png)
![LPF2](https://user-images.githubusercontent.com/45057466/97435898-75e07380-1964-11eb-818a-0b50b6210158.png)

## Android APP

안드로이드 애플리케이션 개발에 관한 설명은 코드에 주석으로 되어있습니다.

[MainActivity.java](https://github.com/GwonHJ/LuggagePickupFast/blob/master/app/src/main/java/com/jhl/ativ/vist/MainActivity.java)

[TaggingActivity.java](https://github.com/GwonHJ/LuggagePickupFast/blob/master/app/src/main/java/com/jhl/ativ/vist/TaggingActivity.java)

[TaggingPauseActivity.java](https://github.com/GwonHJ/LuggagePickupFast/blob/master/app/src/main/java/com/jhl/ativ/vist/TaggingPauseActivity.java)

[LuggageArrivedActivity.java](https://github.com/GwonHJ/LuggagePickupFast/blob/master/app/src/main/java/com/jhl/ativ/vist/LuggageArrivedActivity.java)

[UserTaggingActivity.java](https://github.com/GwonHJ/LuggagePickupFast/blob/master/app/src/main/java/com/jhl/ativ/vist/UserTaggingActivity.java)

[DataCheck.java](https://github.com/GwonHJ/LuggagePickupFast/blob/master/app/src/main/java/com/jhl/ativ/vist/DataCheck.java)

[flyActivity.java](https://github.com/GwonHJ/LuggagePickupFast/blob/master/app/src/main/java/com/jhl/ativ/vist/flyActivity.java)

[LastActivity.java](https://github.com/GwonHJ/LuggagePickupFast/blob/master/app/src/main/java/com/jhl/ativ/vist/LastActivity.java)

[intro.java](https://github.com/GwonHJ/LuggagePickupFast/blob/master/app/src/main/java/com/jhl/ativ/vist/intro.java)

## Spreadsheet

nfc에 카드가 태깅되면 spreadsheet에 저장되게한다. 

spreadsheet에서 정보를 가져와서 firebase에 저장되게한다. 


**1.User** 


[spreadsheet]

<https://docs.google.com/spreadsheets/d/1Ky18cNLftrZCcH0kuFij8dk05H6UuZFmiMSV3MeVAa0/edit?usp=sharing>


```java
function writeDataToFirebase() {
  var sheet = SpreadsheetApp.openById("1Ky18cNLftrZCcH0kuFij8dk05H6UuZFmiMSV3MeVAa0");
  var mysheet = sheet.setActiveSheet(sheet.getSheets()[0]);
  var data = mysheet.getDataRange().getValues();
  var dataToImport = {};
  
  
  var firebaseUrl = "https://vist-rfid.firebaseio.com/";
  var secret = "jDdGbaqymeSbpEgXAty5OHnOI47TDSEqhljxpzRg";
  var base = FirebaseApp.getDatabaseByUrl(firebaseUrl,secret);
  
 
  for(var i = 1; i < data.length; i++) {
    var ID = data[i][1];
    
    dataToImport[ID] = "로그인 필요";
  }
 
  base.setData("User", dataToImport);
}
```


**2.RFID**


[spreadsheet]

<https://docs.google.com/spreadsheets/d/1_BvT5diDqs2XiD1tvIDaTf_a5EBuOgvmQ2MgfDprU8I/edit?usp=sharing>


```java
function writeDataToFirebase() {
  var sheet = SpreadsheetApp.openById("1_BvT5diDqs2XiD1tvIDaTf_a5EBuOgvmQ2MgfDprU8I");
  var mysheet = sheet.setActiveSheet(sheet.getSheets()[0]);
  var data = mysheet.getDataRange().getValues();
  var dataToImport = {};
  
  
  var firebaseUrl = "<https://vist-rfid.firebaseio.com/>";
  var secret = "jDdGbaqymeSbpEgXAty5OHnOI47TDSEqhljxpzRg";
  var base = FirebaseApp.getDatabaseByUrl(firebaseUrl,secret);
  
 
  for(var i = 1; i < data.length; i++) {
    var ID = data[i][1];
    var TIME = data[i][2];
    var NUMBER = data[i][3];
    
    dataToImport[ID] = {
      ID : ID,
      TIME : TIME,
      NUMBER : NUMBER
    };
  }
 
  base.setData("rfid", dataToImport);
}

```

안드로이드 스튜디오 설치 : https://developer.android.com/studio/install?hl=ko

Firebase 주의할 점

    Firebase 이용시 Chrome을 이용하여 접속할 것
    인터넷환경이 좋지 않을 시 접속이 힘들 수 있음
    
Firebase 장점
    
        1. 무료
        2. 보안
        3. 기존의 서버 구축에 비해 간단
        4. 실시간 데이터 처리
        5. 간단한 PUSH 알람
        


