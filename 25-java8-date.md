# Java8 Date

**GMT(Greenwich Mean Time)**\
GMT는 경도 0도에 위치한 영국 런던 그리니치에 있는 왕립 천문대의 시간으로, 모든 시간대의 시작점을 나타내며, 일년내내 DST의 영향을 받지 않는다. GMT는 1925년 2월 5일부터 사용하기 시작했으며, 1972년 1월 1일까지 세계 표준시로 사용되었다.

**UTC(협정 세계시, 協定世界時)**\
UTC는 국제 표준시를 뜻하며 타임존은 아니다. 즉, 공식적으로 UTC를 현지 시간으로 사용하는 국가나 지역은 없다. 협정 세계시를 영어권에서는 Coordinated Universal TimeCUT라고, 프랑스어권에서는 Temps Universel CoordonnéTUC라고 하는데, 혼돈을 방지하기 위해서 공식적으로 UTC라고 정해졌다.

Java 8에서는 타임존을 고려한 날짜와 시간까지도 더 명확하고 편리하게 사용할 수 있다. 타임존은 ZoneId 클래스를 통해 날짜/시간별 DST 이 반영되었는지 확인 할 수도 있다. ZoneId.getAvailableZoneIds() 를 통해 지원하는 지역별 타임존을 확인할 수 있다(현재 시점 등록된 ZoneId는 600개이다).

**DST(Daylight Saving time)**\
DST은 자연 일광을 보다 잘 활용하기 위해서 여름철에 표준 시간에서 1시간 앞으로, 그리고 다시 가을에 시간을 1시간 전으로 설정하는 것을 말한다. DST와 "summer time"은 같은 말을 뜻하며 특정 나라에서 주로 불린다. 영국에서 썸머타임이라고 많이 사용하며, DST가 적용되지 않는 표준시는 "winter time"이라고 사용되기도 한다. DST를 독일에서는 "sommerzeit", 스칸디나비아에서는 "sommertid"라고도 사용한다.

## Classes

### LocalTime/LocalDate/LocalDateTime

시간대(Zone Offset/Zone Region)에 대한 정보가 전혀 없는 API이다.

### ZoneOffset

UTC 기준으로 시간(Time Offset)을 나타낸 것이라고 보면 된다. 우리나라는 KST를 사용하는데 KST는 UTC보다 9시간이 빠르므로 UTC +09:00으로 표기한다. ZoneOffset은 ZoneId의 자식 클래스이다.

* Instant는 글로벌 타임 라인 (UTC)에서 순간적인 지점이며 시간대와 관련이 없습니다.
* LocalDate 및 LocalDateTime에는 표준 시간대 개념이 없지만 now()을 (를) 호출하면 올바른 시간을 얻을 수 있습니다.
* OffsetDateTime에는 시간대가 있지만 일광 절약 시간제를 지원하지 않습니다.
* ZonedDateTime에는 표준 시간대 지원이 있습니다.

### ZoneRegion

Time Zone을 나타낸 것이라고 보면 된다. KST는 타임존의 이름이고 이를 나타내는 ZoneRegion은 Asia/Seoul이다. ZoneRegion은 ZoneId의 자식 클래스이다.

### ZoneRules

ZoneOffset의 UTC +09:00과 ZoneRegion의 Asia/Seoul을 보면 전혀 차이가 없다. 그럼 ZoneOffset과 ZoneRegion은 왜 따로 분리돼있는 걸까? 좀 더 지역에 특화된, 지명 등등을 넣어서 그 의미를 살리고자 분리가 되거나 한 걸까? 이 차이는 DST(Daylight saving time, 서머타임)와 같은 Time Transition Rule을 포함하느냐, 포함하지 않느냐로 갈린다. ZoneOffset은 Time Transition Rule을 포함하지 않는 ZoneRules를 가진다. ZoneRegion은 Time Transition Rule을 포함할 수도, 포함하지 않을 수도 있다.

### OffsetDateTime

LocalDateTime + ZoneOffset에 대한 정보까지 포함한 API이다. 이러한 경우는 축구 경기 생중계 등등에 적합하다.

### ZonedDateTime

OffsetDateTime + ZoneRegion에 대한 정보까지 포함한 API이다. UTC +09:00의 Time Offset을 가지는 Time Zone도 여러가지이다.

* Asia/Seoul
* Asia/Tokyo
* 등등

하지만 시간을 나타내는데 있어서 Asia/Seoul을 쓰던 Asia/Tokyo를 쓰던 큰 차이점이 없다. OffsetDateTime과의 차이점은 DST(Daylight Saving Time)와 같은 Time Transition Rule을 포함하는 ZoneRegion을 갖고 있는 ZoneRules의 유무이다. 독일 등등에서 사용하는 CET(겨울), CEST(여름)는 서머타임을 사용하지 않는 나라에 사는 나 같은 경우에는 굉장히 생소하다. 그래서 어떤 때는 CET를 사용해야하고, 어떤 때는 CEST를 사용해야할지 매우 애매하고 계산하기도 까다롭다. 자바에서는 이 두 Time Zone을 하나의 Time Zone(CET)로 통일하고 Time Transition Rule을 가지는 ZoneRules를 통해 알아서 내부적으로 계산해준다.

### Instant

어느 순간을 나타내는 클래스이다. Unix Timestamp를 구할 때 사용한다. UTC 기준이기 때문에 글로벌한 서비스에서도 매우 적합할 것이다.

## JRE 및 DST 가변성

첫째, 전 세계 DST 영역이 매우 자주 변경 되며이를 조정하는 중앙 기관이 없다는 사실을 이해하는 것이 매우 중요 합니다.

국가 또는 경우에 따라 도시에서 신청 또는 취소 여부와 방법을 결정할 수 있습니다.

변경 사항이 발생할 때마다 [**IANA 시간대**](https://www.iana.org/time-zones) 데이터베이스에 기록되고 업데이트는 JRE 의 향후 릴리스 에서 롤아웃됩니다 .

기다릴 수없는 경우 Java SE 다운로드 페이지 에서 사용할 수있는 Java Time Zone Updater Tool 이라는 공식 Oracle 도구를 통해 새 DST 설정이 포함 된 수정 된 시간대 데이터를 JRE로 강제 적용 할 수 있습니다 .

> 라이선스가 없으면 Java Time Zone Updater Tool은 사용할 수 없음. \*\* 더 확인해 보아야 함 \*\*

## 올바른 TZDB 시간대 아이디

Java에서 DST를 처리하는 올바른 방법 은 "Europe/Rome"와 같은 특정 TZDB Timezone ID로 시간대를 인스턴스화하는 것 입니다.

## 현재의 로컬 시간을 구한다.

```java
	@Test
	public void testCurrentDateTime() {
		LocalDateTime localDateTime  = LocalDateTime.now();
		System.out.println(localDateTime);
		// 2019-09-25T15:50:43.265
	}
```

## 모든 ZoneID를 출력

```java
	/**
	 * 모든 ZoneId를 출력한다.
	 */
	@Test
	public void testPrintAllZoneIds() {
		Set<String> allZoneIds = ZoneId.getAvailableZoneIds();
		allZoneIds.forEach(id -> System.out.println(id));
  }
```

> java.time.ZonedDateTime 클래스는 영역 세부 정보로 datetime 객체를 처리하는 데 도움이됩니다. 가장 중요한 것은 java.time.ZonedDateTime 클래스가 DST를 완전히 인식 하고 있으므로 어깨에서 부담을 덜어줍니다.






```shell
// 로컬 시간을 의미하는 ISO 8601 문자열
2021-10-06T15:00:00.000
// UTC(GMT) 시간을 의미하는 ISO 8601 문자열
2021-10-06T06:00:00.000Z

// 로컬 시간을 의미하면서 UTC(GMT) 대비 +09:00 임을 의미하는 ISO 8601 문자열
2021-10-06T15:00:00.000+09:00
```
- `2021-10-06T15:00:00.000`은 **ISO 8601**의 기본 형식이다. 해당 시간이 로컬 시간 임을 의미한다.
- `2021-10-06T06:00:00.000Z`와 같이 뒤에 `Z` 식별자를 추가하면 해당 시간이 **UTC(GMT)** 시간 임을 의미한다.
- `2021-10-06T15:00:00.000+09:00`와 같이 뒤에 **Z** 대신 `+HH:mm` 식별자를 추가하면 해당 시간이 로컬 시간이면서 **UTC(GMT)**와 **09:00** 만큼 차이가 남을 의미한다. 이 형식의 장점은 인간이 손쉽게 추가적인 계산 없이 로컬 시간을 인지하면서 추가적으로 타임존 정보까지 제공하기 때문에 가장 인간친화적이라고 할 수 있다.




## References

[날짜와 시간 API with Java8](https://wickso.me/java/java-8-date-time/)\
[(Java8) 날짜와 시간 API](https://perfectacle.github.io/2018/09/26/java8-date-time/) [Java에서 일광 절약 시간을 처리하는 방법](http://www.programmergirl.com/handle-day-light-savings-java/)
