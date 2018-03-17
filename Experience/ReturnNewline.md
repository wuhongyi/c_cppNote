<!-- ReturnNewline.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 六 3月 17 16:37:16 2018 (+0800)
;; Last-Updated: 六 3月 17 17:01:42 2018 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 3
;; URL: http://wuhongyi.cn -->

# 回车符和换行符

```text
Unix系统里，每行结尾只有“<换行>”，即“\n”；
Windows系统里面，每行结尾是“<换行><回车>”，即“\r\n”；
Mac系统里，每行结尾是“<回车>”。
一个直接后果是，Unix/Mac系统下的文件在Windows里打开的话，所有文字会变成一行；
而Windows里的文件在Unix/Mac下打开的话，在每行的结尾可能会多出一个^M符号。

Linux中遇到换行符("\n")会进行回车+换行的操作，回车符反而只会作为控制字符("^M")显示，不发生回车的操作。而windows中要回车符+换行符("\r\n")才会回车+换行，缺少一个控制符或者顺序不对都不能正确的另起一行。
```


```cpp
// main.cc --- 
// 
// Description: 
// Author: Hongyi Wu(吴鸿毅)
// Email: wuhongyi@qq.com 
// Created: 日 2月 25 14:33:00 2018 (+0800)
// Last-Updated: 六 3月 17 16:55:47 2018 (+0800)
//           By: Hongyi Wu(吴鸿毅)
//     Update #: 24
// URL: http://wuhongyi.cn 

#include "RVersion.h"//版本判断
#include "TApplication.h"
#include "TArrow.h"
#include "TAxis.h"
#include "TBenchmark.h"
#include "TBranch.h"
#include "TBrowser.h"
#include "TCanvas.h"
#include "TChain.h"
#include "TColor.h"
#include "TCutG.h"
#include "TDatime.h"
#include "TError.h"
#include "TF1.h"
#include "TF2.h"
#include "TFile.h"
#include "TFitResult.h"
#include "TFormula.h"
#include "TGaxis.h"
#include "TGraph.h"
#include "TGraph2D.h"
#include "TGraphErrors.h"
#include "TH1.h"
#include "TH2.h"
#include "TH3.h"
#include "THStack.h"
#include "TLatex.h"
#include "TLegend.h"
#include "TLegendEntry.h"
#include "TLine.h"
#include "TList.h"
#include "TLorentzVector.h"
#include "TMarker.h"
#include "TMath.h"
#include "TMatrixD.h"
#include "TMatrixDEigen.h"
#include "TMultiGraph.h"
#include "TNtuple.h"
#include "TObject.h"
#include "TPad.h"
#include "TPaveLabel.h"
#include "TPaveStats.h"
#include "TPaveText.h"
#include "TRandom.h"
#include "TRandom1.h"
#include "TRandom2.h"
#include "TRandom3.h"
#include "TRint.h"
#include "TROOT.h"
#include "TSlider.h"
#include "TSpectrum.h"
#include "TSpectrum2.h"
#include "TStopwatch.h"
#include "TString.h"
#include "TStyle.h"
#include "TSystem.h"
#include "TTimer.h"
#include "TTimeStamp.h"
#include "TTree.h"
#include "TVector3.h"
#include "TVectorD.h"

// #define NDEBUG
#include <algorithm>
#include <cassert>
#include <cfloat>
#include <climits>
#include <cmath>
#include <complex>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <ctime>
#include <deque>
#include <fstream>
#include <iomanip>
#include <iostream>
#include <list>
#include <map>
#include <queue>
#include <set>
#include <sstream>
#include <stack>
#include <string>
#include <vector>

//....oooOO0OOooo........oooOO0OOooo........oooOO0OOooo........oooOO0OOooo......

int main(int argc, char *argv[])
{
  // Create an interactive ROOT application
  TRint *theApp = new TRint("Rint", &argc, argv);

  TCanvas *c1 = new TCanvas("c1","",600,400);

  double tau = 2000;

  TGraph *g1 = new TGraph();
  // g1 = new TGraph(n, x, y);//int float double
  g1->SetTitle("");
  g1->GetXaxis()->SetTitle("");
  g1->GetYaxis()->SetTitle("");
  // g1->SetLineColor(Color_t);
  // g1->SetLineStyle(Style_t);
  // g1->SetLineWidth(Width_t);//1
  // g1->SetFillStyle(Style_t);
  // g1->SetFillColor(Color_t);
  // g1->SetMarkerColor(Color_t);
  // g1->SetMarkerStyle(Style_t);
  // g1->SetMarkerSize(Size_t);
  // g1->SetMinimum(Double_t);
  // g1->SetMaximum(Double_t);
  // g1->IsInside(Double_t x, Double_t y);// int   判断（x，y）是否在TCut选定的范围
  // g1->Eval(Double_t x);//double 获得x对应的y值
  // g1->GetN();//int 获得点数


  for (int i = 0; i < 10000; ++i)
    {
      if(i < 1000)
	{
	  g1->SetPoint(i, i, 2000+gRandom->Gaus(0, 30));//i = 0~N-1

	}
      else
	{
	  g1->SetPoint(i, i, 2000+gRandom->Gaus(0, 30)+12000*TMath::Exp(-(i-1000)/tau));

	}
    }
  
  g1->Draw("A*");
  c1->Update();


  std::ofstream writedata;//fstream
  writedata.open("waveform.coe");//ios::bin ios::app
  if(!writedata.is_open())
    {
      std::cout<<"can't open file."<<std::endl;
    }
  writedata<<"MEMORY_INITIALIZATION_RADIX=16;"<<"\r\n";
  writedata<<"MEMORY_INITIALIZATION_VECTOR="<<"\r\n";


  for (int i = 0; i < 10000; ++i)
    {
      // std::cout<<std::dec<<i<<"  "<<std::hex<<std::setw(4)<<std::setfill('0') <<int(g1->GetY()[i])<<std::endl;
      if(i==9999)
	{
	  writedata<<std::hex<<std::setw(4)<<std::setfill('0') <<int(g1->GetY()[i])<<";"<<"\r\n";
	}
      else
	{
	  writedata<<std::hex<<std::setw(4)<<std::setfill('0') <<int(g1->GetY()[i])<<","<<"\r\n";
	}
    }

  writedata.close();

  // and enter the event loop...
  theApp->Run();
  // delete theApp;

  return 0;
}


// 
// main.cc ends here
```



----

> http://blog.csdn.net/xiaofei2010/article/details/8458605

<!-- ReturnNewline.md ends here -->
