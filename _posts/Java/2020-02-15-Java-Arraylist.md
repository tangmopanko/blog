---
title: "[자료구조] Arraylist"
layout: posts
excerpt: "Arraylist"
search: true
categories:
  - DataStructure
tags:
  - 자바
  - 자료구조
---
## Arraylist

- 동적 배열이며, 새 요소가 추가되면 크기가 커지고, 제거되면 줄어든다. 

- 내부적으로 배열을 사용하여 저장하기에 인덱스로 요소를 검색 할 수 있다. 

- 중복 및 널 값을 허용한다 

- 정렬된 Collection이기에 삽입 순서를 유지한다. 

- 기본적으로 동기화되지 않기에 동기화를 하기 위해 명시적으로 동기화 해야한다. 

```java
        List<String> datas = new ArrayList<String>();
        datas.add("TV");
        datas.add("Radio");
        datas.add("Audio");
        datas.addAll(Arrays.asList("Cellphone", "Microwave"));
        datas.add(datas.size(), "Computer");
        // TV, Radio, Audio, Cellphone, Microwave, Computer

        // 반복자
        Iterator<String> dataIter = datas.iterator();

        String data = "";
        while (dataIter.hasNext()) {
            data = dataIter.next();
            System.out.println(data);
        }

        ListIterator<String> dataPreIter = datas.listIterator(datas.size());
        while (dataPreIter.hasPrevious()) {
            data = dataPreIter.previous();
            System.out.println(data);
        }

        datas.forEach(appliance -> {
            System.out.print(appliance);
        });

        datas.sort(new Comparator<String>() {
            @Override
            public int compare(String name1, String name2) {
                return name1.compareTo(name2); //
            }
        });

        datas.forEach(appliance -> {
            System.out.println(appliance);
        });

        datas.sort((Comparator.naturalOrder()));

        // Remove TV
        datas.removeIf(new Predicate<String>() {
            @Override
            public boolean test(String s) {
                return s.startsWith("T");
            }
        });

        // Remove all elements
        datas.clear();

        // synchronized CollectionList
        // Arraylist 클래스는 동기화기능을 지원하지 않기에 이를 해결하기 위해 아래와 같이 작성한다.
        List<Integer> saftyArrayList = Collections.synchronizedList(Arrays.asList(1, 2, 3, 4, 5, 6));
```



