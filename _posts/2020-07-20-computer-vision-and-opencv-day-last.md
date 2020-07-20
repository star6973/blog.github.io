---
layout: post
title: Computer Vision and OpenCV [Day Last]
date: 2020-07-20 10:00:00-19:00:00
categories: OpenCV
tag: OpenCV
use_math: true
---

```cython
#include "histo.hpp"

Mat query_img()
{
   do {
      int q_no = 74;
      cout << "질의 영상 번호를 입력하세요 : ";
      cin >> q_no;

      String  fname = format("../image/DB/img_%02d.jpg", q_no);
      Mat query = imread(fname, IMREAD_COLOR);

      if (query.empty())   cout << "질의 영상 번호가 잘못되었습니다." << endl;
      else             return query;
   } while (1);
}

int main()
{
   Vec3i bins(30, 42, 0);
   Vec3f ranges(180, 256, 0);
   vector<Mat> DB_hists = load_histo(bins, ranges, 100);
   
   for (int i = 0; i < 100; i++) {
      Mat hist_img = draw_histo(DB_hists[i]);
      imshow("hist_img" + i, hist_img);
   }
   
   
   Mat query = query_img(); // 숫자 입력을 통한 영상 파일 입력 받기
   Mat hsv, query_hist;

   // HSV 컬러 변환
   cvtColor(query, hsv, COLOR_BGR2HSV);
   // 2차원 히스토그램 계산
   calc_Histo(hsv, query_hist, bins, ranges, 2);
   // 2차원 그래프
   Mat hist_img = draw_histo(query_hist);

   imshow("query", query);
   imshow("hist_img", hist_img);
   //   imshow("query + hist_img", make_img(query, hist_img));

   waitKey();
   return 0;
}
```

```cython
#include "histo.hpp"
#include "utils.hpp"

int main()
{
   Vec3i bins(30, 42, 0);
   Vec3f ranges(180, 256, 0);

   vector<Mat> DB_hists = load_histo(bins, ranges, 100);
   Mat query = query_img();

   Mat hsv, query_hist;
   cvtColor(query, hsv, CV_BGR2HSV);               // HSV 컬러 변환
   calc_Histo(hsv, query_hist, bins, ranges, 2);
   Mat hist_img = draw_histo(query_hist);

   // 쿼리값이랑 DB에 저장된 값이랑 가장 비슷한 것을 찾기
   Mat DB_similaritys = calc_similarity(query_hist, DB_hists);

   double  sinc;
   cout << "   기준 유사도 입력: ";
   cin >> sinc;
   select_view(sinc, DB_similaritys);
   //   select_view(sinc, DB_similaritys, bins, ranges);

   imshow("image", query);
   imshow("hist_img", hist_img);
   waitKey();

   return 0;
}
```