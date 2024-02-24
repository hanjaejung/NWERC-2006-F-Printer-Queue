# NWERC-2006-F-Printer-Queue
NWERC 2006 F Printer Queue
package com.example.javastudy.codingStudy;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Collections;
import java.util.LinkedList;
import java.util.Queue;

public class Main {

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String str = br.readLine();    //검사할 총 개수
        int counts = Integer.parseInt(str);
        StringBuffer bf = new StringBuffer();

        while(counts > 0) {
            String str1 = br.readLine();    //문자열 길이 및 검사할 번수
            String[] arr = str1.split(" ");
            int length = Integer.parseInt(arr[0]);
            int value =      Integer.parseInt(arr[1]);

            String str2 = br.readLine();    //검사할 문자열
            String[] arr1 = str2.split(" ");
            Queue<Integer> queue = new LinkedList<Integer>();
            for(int i =0; i < length; i++){
                queue.offer(Integer.parseInt(arr1[i]));
            }

            int count = 0;

            boolean bl = false;
            int deleteCount = 0;
            while (!queue.isEmpty()) {

                    if (Collections.max(queue) > queue.peek()) {  //멘처음값이 뒤에 있는 값 중 최대값이 존재할시 맨 처음 값을 뒤로 보내기
                        queue.offer(queue.poll());

                        if(count == value && !bl) { //위치값 걸릴때 어차피 맨뒤로 가니 큐 사이즈 만큼 위치값 조정
                            value = queue.size();
                            bl = true;
                        }else if(bl){ //위치값 맨뒤로 가면서 이 후에 또다른 단어가 뒤로 갈시 한자리만 위치 마이너스
                            value--;
                        }

                        if(value == 0 && bl){ //도는 동안 삭제가 안됐을 경우 다시 맨뒤로 갈시 사이즈 만큼 위치 조정
                            value = queue.size();
                        }

                    } else if (Collections.max(queue) == queue.peek()) { //이 부분은 큐의 최대값과 제일 앞에 있는값이 같을 경우 삭제

                        deleteCount++; //위치 값은 계속 줄어드는 상황이라 삭제부터는 카운터를 쌓아야 한다

                        if(bl) {
                            value--; //위치값 걸리지전 마찬가지로 하나 삭제 하니 라인도 하나 줄어듬
                        }

                        if(value == 0 && bl){ //bl == true 찾는 값이 아직 안에 있을때 0이면 맨앞자리까지 온거므로 더이상 검사할게 없다
                            value = deleteCount; //그래서 삭제 된 개수를 최종값에 넣는다
                            while(!queue.isEmpty()){ //while문을 빠져나가기 위해 큐값을 다 뺸다
                                queue.poll();
                            }
                        }

                        if(value == count && !bl){ //1) 위 조건문에서 뒤로 보낸값이 단 한번도 없다면 몇번쨰 정한값과 카운트값이 일치핢때
                            value = deleteCount; //2) 거기서 삭제 한값이 몇번째 삭제한 값이다
                            while(!queue.isEmpty()){ //while문을 빠져나가기 위해 큐값을 다 뺸다
                                queue.poll();
                            }
                        }
                        queue.poll();
                    }
                count++;
            }

            if(queue.isEmpty()){
                bf.append(value).append("\n");
            }
            counts--;
        }
        System.out.println(bf);

    }
}
