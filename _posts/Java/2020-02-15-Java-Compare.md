---
title: "[Interface] Comparable"
layout: posts
excerpt: "Comparable, Comparator"
search: true
categories:
  - Interface
tags:
  - 자바
  - 인터페이스
---
## Comparable, Comparator

- Comparable - 정렬 기준이 되는 메소드를 정의함. 
- Comparator - 기존방식이 아닌 다른 방식으로 정렬하고 싶을때 사용. 

```java

public class CompableTest {
        public static void main(String... args) {
            List<Music> musicList = new ArrayList<>();
            musicList.add(new Music(2, "트와이스_YES or YES", 24000, LocalDate.of(2019, 11, 5)));
            musicList.add(new Music(1, "아이유_GrowingUp", 22000, LocalDate.of(2009, 4, 23)));
            musicList.add(new Music(3, "오마이걸_비밀정원", 17000, LocalDate.of(2018, 9, 10)));

            musicList.forEach(music -> {
                System.out.print(music.getId()); // 2 1 3
            });

            Collections.sort(musicList);
            // [Music {id= 1, name= 아이유_GrowingUp, price= 22000.0, releaseDate= 2009-04-23}
            // , Music {id= 2, name= 트와이스_YES or YES, price= 24000.0, releaseDate= 2019-11-05}
            // , Music {id= 3, name= 오마이걸_비밀정원, price= 17000.0, releaseDate= 2018-09-10}]

            Collections.sort(musicList, Comparator.comparing(Music::getName));
            // [Music {id= 1, name= 아이유_GrowingUp, price= 22000.0, releaseDate= 2009-04-23}
            // , Music {id= 3, name= 오마이걸_비밀정원, price= 17000.0, releaseDate= 2018-09-10}
            // , Music {id= 2, name= 트와이스_YES or YES, price= 24000.0, releaseDate= 2019-11-05}]

            Collections.sort(musicList, Comparator.comparing(Music::getName).reversed());
            // [Music {id= 2, name= 트와이스_YES or YES, price= 24000.0, releaseDate= 2019-11-05}
            // , Music {id= 3, name= 오마이걸_비밀정원, price= 17000.0, releaseDate= 2018-09-10}
            // , Music {id= 1, name= 아이유_GrowingUp, price= 22000.0, releaseDate= 2009-04-23}]

            Collections.sort(musicList, Comparator.comparing(Music::getReleaseDate));
            // [Music {id= 1, name= 아이유_GrowingUp, price= 22000.0, releaseDate= 2009-04-23}
            // , Music {id= 3, name= 오마이걸_비밀정원, price= 17000.0, releaseDate= 2018-09-10}
            // , Music {id= 2, name= 트와이스_YES or YES, price= 24000.0, releaseDate= 2019-11-05}]

            List<Music> sortedMusicList = musicList.stream().sorted((a, b) -> (int) (a.getPrice() - b.getPrice()))
                    .collect(Collectors.toList());
            // [Music {id= 3, name= 오마이걸_비밀정원, price= 17000.0, releaseDate= 2018-09-10}
            // , Music {id= 1, name= 아이유_GrowingUp, price= 22000.0, releaseDate= 2009-04-23}
            // , Music {id= 2, name= 트와이스_YES or YES, price= 24000.0, releaseDate= 2019-11-05}]
            
        }
				
  			// Comparable 상속받아 구현하고 compareTo을 이용해 정렬기준을 생성.  
        static class Music implements Comparable<Music> {
            private int id;
            private String name;
            private double price;
            private LocalDate releaseDate;

            // id로 비교
            @Override
            public boolean equals(Object obj) {
                if (this == obj) return true;
                if (obj == null || getClass() != obj.getClass()) return false;
                Music music = (Music) obj;
                return id == music.id;
            }

            @Override
            public String toString() {
                return "Music {" +
                        "id= " + id +
                        ", name= " + name +
                        ", price= " + price +
                        ", releaseDate= " + releaseDate +
                        "}";
            }

            @Override
            public int hashCode() {
                return Objects.hashCode(id);
            }

            public Music(int id, String name, double price, LocalDate releaseDate) {
                this.id = id;
                this.name = name;
                this.price = price;
                this.releaseDate = releaseDate;
            }

            @Override
            public int compareTo(Music o) {
                return this.getId() - o.getId();
            }

            public int getId() {
                return id;
            }

            public String getName() {
                return name;
            }

            public double getPrice() {
                return price;
            }

            public LocalDate getReleaseDate() {
                return releaseDate;
            }
        }
}

```



