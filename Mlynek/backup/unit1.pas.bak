unit Unit1;

{$mode objfpc}{$H+}

interface

uses
  Classes, SysUtils, db, FileUtil, Forms, Controls, Graphics, Dialogs, StdCtrls,
  ExtCtrls;

type

  { TForm1 }

  TForm1 = class(TForm)
    Button1: TButton;
    Button2: TButton;
    procedure Zamykanie(Sender: TObject);
    procedure Rysowanie(Sender: TObject);
    procedure Shape1MouseUp(Sender: TObject; Button: TMouseButton;
      Shift: TShiftState; X, Y: Integer);
    procedure Shape1StartDrag(Sender: TObject; var DragObject: TDragObject);
  private
    { private declarations }
  public
    { public declarations }
  end;

var
  Form1: TForm1;
  tab : array [0..41,1..4] of integer;
  possibleMoves : array [0..23,0..3] of integer;
  pola : array [0 .. 41] of integer;
  i, j, zm, greyFiguresOnBoardAmount :integer;
  whiteMoveNext : Boolean;

implementation

{$R *.lfm}

procedure wspolrzedne();
begin
  for i:=0 to 2 do
  begin
    tab[i,1]:=85+i*100; tab[i,2]:=80+i*100; tab[i,3]:=i*100+125; tab[i,4]:=i*100+120;
    tab[i+3,1]:=i*100+85; tab[i+3,2]:=334; tab[i+3,3]:=i*100+125; tab[i+3,4]:=374;
    tab[i+6,1]:=i*100+85; tab[i+6,2]:=580-i*100; tab[i+6,3]:=i*100+125; tab[i+6,4]:=620-i*100;

    tab[i+9,1]:=585-i*100; tab[i+9,2]:=80+i*100; tab[i+9,3]:=625-i*100; tab[i+9,4]:=120+i*100;
    tab[i+12,1]:=585-i*100; tab[i+12,2]:=334; tab[i+12,3]:=625-i*100; tab[i+12,4]:=374;
    tab[i+15,1]:=585-i*100; tab[i+15,2]:=580-i*100; tab[i+15,3]:=625-i*100; tab[i+15,4]:=620-i*100;

    tab[i+18,1]:=335; tab[i+18,2]:=80+i*100; tab[i+18,3]:=375; tab[i+18,4]:=120+i*100;
    tab[i+21,1]:=335; tab[i+21,2]:=580-i*100; tab[i+21,3]:=375; tab[i+21,4]:=620-i*100;
  end;

  for i:=0 to 8 do
  begin
    tab[i+24,1]:=85+i*62; tab[i+24,2]:=650; tab[i+24,3]:=125+i*62; tab[i+24,4]:=690;
    tab[i+33,1]:=85+i*62; tab[i+33,2]:=720; tab[i+33,3]:=125+i*62; tab[i+33,4]:=760;
  end;
end;

procedure determinPossibleMoves();
begin
  possibleMoves[0, 0] := 3; possibleMoves[0, 1] := 18; possibleMoves[0, 2] := -1; possibleMoves[0, 3] := -1;
  possibleMoves[1, 0] := 5; possibleMoves[1, 1] := 19; possibleMoves[1, 2] := -1; possibleMoves[1, 3] := -1;
  // dodaj reszte mozliwosci
end;

procedure biale_dodatkowe(canvas : Tcanvas);
begin
  with canvas do begin
  brush.color:=clwhite;

    for j:=0 to 8 do
    begin
      ellipse (tab[j+24,1], tab[j+24,2], tab[j+24,3], tab[j+24,4]);
      pola[j+24]:=1;
    end;
end;

end;

{ TForm1 }

procedure TForm1.Rysowanie(Sender: TObject);
begin
  wspolrzedne();
  determinPossibleMoves();
  zm:=1;
  greyFiguresOnBoardAmount:=24;
  whiteMoveNext := True;
  button1.visible:=false;
  Form1.Repaint;
  with canvas do begin
    brush.color:=clgray;
    for j:=1 to 3 do
    begin
      rectangle(j*100,j*100,j*100+10,(1-j)*100+600);
      rectangle(j*100,j*100,(1-j)*100+600,j*100+10);
      rectangle((1-j)*100+600,(1-j)*100+600,(1-j)*100+610, j*100);
      rectangle(j*100,(1-j)*100+600, (1-j)*100+610, (1-j)*100+610);
    end;

    rectangle(100,350,300,360);
    rectangle(350,100,360,300);
    rectangle(400,350,600,360);
    rectangle(350,400,360,600);

    for j:=0 to 23 do
    begin
        ellipse (tab[j,1], tab[j,2], tab[j,3], tab[j,4]);
    end;

    brush.color:=clwhite;

    for j:=0 to 8 do
    begin
      ellipse (tab[j+24,1], tab[j+24,2], tab[j+24,3], tab[j+24,4]);
      pola[j+24]:=1;
    end;

    brush.color:=clblack;
    for j:=0 to 8 do
    begin
      ellipse (tab[j+33,1], tab[j+33,2], tab[j+33,3], tab[j+33,4]);
      pola[j+33]:=2;
    end;
  end;
end;

procedure TForm1.Zamykanie(Sender: TObject);
begin
  close();
end;

procedure isNonRed(initialFigureNo, loopEnd: Integer; Out nonRed : Boolean);
begin
     nonRed := True;
      for i:=0 to loopEnd do begin
       if (pola[i+initialFigureNo] = 3) then begin
            nonRed := False;
            Break;
       end;
  end  ;
end;

procedure countOfGreyFiguresOnBoard(Out count : Integer);
begin
  count:=0;
  for i:=0 to 23 do begin
      if (pola[i] = 0) then begin
            inc(count);
       end;
  end;
end;

procedure TForm1.Shape1MouseUp(Sender: TObject; Button: TMouseButton;
  Shift: TShiftState; X, Y: Integer);
var nonRedFigure, nonRedAll, nextPlayer, nextMovePossible : Boolean;
var greysCount : Integer;
begin
  wspolrzedne();
  isNonRed(24, 17, nonRedFigure);
  isNonRed(0, 41, nonRedAll);
  countOfGreyFiguresOnBoard(greysCount);

  nextPlayer := False;
  if greysCount < greyFiguresOnBoardAmount then begin
       greyFiguresOnBoardAmount := greysCount;
       nextPlayer := True;
  end;

  if nonRedAll and nextPlayer then begin
       whiteMoveNext := not whiteMoveNext;
  end;

  nextMovePossible := False;

  // pobierz indeks obecnie czerwonego kolka (X)
  // sprawdzone ze klikniecie jest w kolko i pobierz jego indeks (Y)
  // possibleMoves[X] -> tablica indeksow / to twoje mozliwe ruchy
  // jesli ktorys z indeksow tablicy zwrocenej powyzej jest rowny Y to ruch jest mozliwy to nextMovePossible = True;
  // if nextMovePossible then Wszystko inne czyli zamalowac na szaro obecny czerwone kolko X i obecny kolor przemalowac na docelowy Y

if whiteMoveNext then begin
  begin
  with canvas do
  begin
  for i:=0 to 8 do
  begin
  if (pola[i+24]=3) and (x>85+i*62) and (y>650) and (x<125+i*62) and (y<690) then
  begin
  brush.color:=clwhite;
  ellipse(tab[i+24,1],tab[i+24,2],tab[i+24,3],tab[i+24,4]);
  pola[i+24]:=1;
  end
  else

  if (pola[i+24]=1) and nonRedFigure and (x>tab[i+24,1]) and (y>tab[i+24,2]) and (x<tab[i+24,3]) and (y<tab[i+24,4]) then
  begin
    brush.color:=clred;
    ellipse(tab[i+24,1],tab[i+24,2],tab[i+24,3],tab[i+24,4]);
    pola[i+24]:=3;
    end;
  end;
  end;

  with canvas do begin

    if (pola[24]=3)  then
  begin
    for i:=0 to 23 do begin
    if (pola[i]<>2) and (pola[i]<>1) and (x>tab[i,1]) and (y>tab[i,2]) and (x<tab[i,3]) and (y<tab[i,4]) then
    begin
    brush.color:=clwhite;
    ellipse(tab[i,1],tab[i,2],tab[i,3],tab[i,4]);
    pola[24]:=0;
    pola[i]:=1;
    brush.color:=clgray;
    ellipse(tab[24,1],tab[24,2],tab[24,3],tab[24,4]);

    end;
    end;
    end;

    if (pola[25]=3) then
    begin
    for i:=0 to 23 do begin
    if (pola[i]<>2) and (pola[i]<>1) and (x>tab[i,1]) and (y>tab[i,2]) and (x<tab[i,3]) and (y<tab[i,4]) then
    begin
    brush.color:=clwhite;
    ellipse(tab[i,1],tab[i,2],tab[i,3],tab[i,4]);
    pola[25]:=0;
    pola[i]:=1;
    brush.color:=clgray;
    ellipse(tab[25,1],tab[25,2],tab[25,3],tab[25,4]);
    end;
    end;
    end;

     if (pola[26]=3) then
    begin
    for i:=0 to 23 do begin
    if (pola[i]<>2) and (pola[i]<>1) and (x>tab[i,1]) and (y>tab[i,2]) and (x<tab[i,3]) and (y<tab[i,4]) then
    begin
    brush.color:=clwhite;
    ellipse(tab[i,1],tab[i,2],tab[i,3],tab[i,4]);
    pola[26]:=0;
    pola[i]:=1;
    brush.color:=clgray;
    ellipse(tab[26,1],tab[26,2],tab[26,3],tab[26,4]);
    end;
    end;
    end;

    if (pola[27]=3) then
    begin
    for i:=0 to 23 do begin
    if (pola[i]<>2) and (pola[i]<>1) and (x>tab[i,1]) and (y>tab[i,2]) and (x<tab[i,3]) and (y<tab[i,4]) then
    begin
    brush.color:=clwhite;
    ellipse(tab[i,1],tab[i,2],tab[i,3],tab[i,4]);
    pola[27]:=0;
    pola[i]:=1;
    brush.color:=clgray;
    ellipse(tab[27,1],tab[27,2],tab[27,3],tab[27,4]);
    end;
    end;
    end;

    if (pola[28]=3) then
    begin
    for i:=0 to 23 do begin
    if (pola[i]<>2) and (pola[i]<>1) and (x>tab[i,1]) and (y>tab[i,2]) and (x<tab[i,3]) and (y<tab[i,4]) then
    begin
    brush.color:=clwhite;
    ellipse(tab[i,1],tab[i,2],tab[i,3],tab[i,4]);
    pola[28]:=0;
    pola[i]:=1;
    brush.color:=clgray;
    ellipse(tab[28,1],tab[28,2],tab[28,3],tab[28,4]);
    end;
    end;
    end;

    if (pola[29]=3) then
    begin
    for i:=0 to 23 do begin
    if (pola[i]<>2) and (pola[i]<>1) and (x>tab[i,1]) and (y>tab[i,2]) and (x<tab[i,3]) and (y<tab[i,4]) then
    begin
    brush.color:=clwhite;
    ellipse(tab[i,1],tab[i,2],tab[i,3],tab[i,4]);
    pola[29]:=0;
    pola[i]:=1;
    brush.color:=clgray;
    ellipse(tab[29,1],tab[29,2],tab[29,3],tab[29,4]);
    end;
    end;
    end;

    if (pola[30]=3) then
    begin
    for i:=0 to 23 do begin
    if (pola[i]<>2) and (pola[i]<>1) and (x>tab[i,1]) and (y>tab[i,2]) and (x<tab[i,3]) and (y<tab[i,4]) then
    begin
    brush.color:=clwhite;
    ellipse(tab[i,1],tab[i,2],tab[i,3],tab[i,4]);
    pola[30]:=0;
    pola[i]:=1;
    brush.color:=clgray;
    ellipse(tab[30,1],tab[30,2],tab[30,3],tab[30,4]);
    end;
    end;
    end;

    if (pola[31]=3) then
    begin
    for i:=0 to 23 do begin
    if (pola[i]<>2) and (pola[i]<>1) and (x>tab[i,1]) and (y>tab[i,2]) and (x<tab[i,3]) and (y<tab[i,4]) then
    begin
    brush.color:=clwhite;
    ellipse(tab[i,1],tab[i,2],tab[i,3],tab[i,4]);
    pola[31]:=0;
    pola[i]:=1;
    brush.color:=clgray;
    ellipse(tab[31,1],tab[31,2],tab[31,3],tab[31,4]);
    end;
    end;
    end;

    if (pola[32]=3) then
    begin
    for i:=0 to 23 do begin
    if (pola[i]<>2) and (pola[i]<>1) and (x>tab[i,1]) and (y>tab[i,2]) and (x<tab[i,3]) and (y<tab[i,4]) then
    begin
    brush.color:=clwhite;
    ellipse(tab[i,1],tab[i,2],tab[i,3],tab[i,4]);
    pola[32]:=0;
    pola[i]:=1;
    brush.color:=clgray;
    ellipse(tab[32,1],tab[32,2],tab[32,3],tab[32,4]);
    end;
    end;
    end;


  end;
      end;
end
   else

begin

  with canvas do
  begin
  for i:=0 to 8 do
  begin
  if (pola[i+33]=3) and (x>85+i*62) and (y>720) and (x<125+i*62) and (y<760) then
  begin
  brush.color:=clblack;
  ellipse(tab[i+33,1],tab[i+33,2],tab[i+33,3],tab[i+33,4]);
  pola[i+33]:=2;
  end
  else

  if (pola[i+33]=2) and nonRedFigure and (x>tab[i+33,1]) and (y>tab[i+33,2]) and (x<tab[i+33,3]) and (y<tab[i+33,4]) then
  begin
    brush.color:=clred;
    ellipse(tab[i+33,1],tab[i+33,2],tab[i+33,3],tab[i+33,4]);
    pola[i+33]:=3;
  end;
  end;
  end;


  with canvas do begin



  if (pola[33]=3) then
  begin
    for i:=0 to 23 do begin
    if (pola[i]<>2) and (pola[i]<>1) and (x>tab[i,1]) and (y>tab[i,2]) and (x<tab[i,3]) and (y<tab[i,4]) then
    begin
    brush.color:=clblack;
    ellipse(tab[i,1],tab[i,2],tab[i,3],tab[i,4]);
    pola[33]:=0;
    pola[i]:=2;
    brush.color:=clgray;
    ellipse(tab[33,1],tab[33,2],tab[33,3],tab[33,4]);
    end;
    end;
  end;

  if (pola[34]=3) then
  begin
    for i:=0 to 23 do begin
    if (pola[i]<>2) and (pola[i]<>1) and (x>tab[i,1]) and (y>tab[i,2]) and (x<tab[i,3]) and (y<tab[i,4]) then
    begin
    brush.color:=clblack;
    ellipse(tab[i,1],tab[i,2],tab[i,3],tab[i,4]);
    pola[34]:=0;
    pola[i]:=2;
    brush.color:=clgray;
    ellipse(tab[34,1],tab[34,2],tab[34,3],tab[34,4]);
    end;
    end;
  end;

  if (pola[35]=3) then
  begin
    for i:=0 to 23 do begin
    if (pola[i]<>2) and (pola[i]<>1) and (x>tab[i,1]) and (y>tab[i,2]) and (x<tab[i,3]) and (y<tab[i,4]) then
    begin
    brush.color:=clblack;
    ellipse(tab[i,1],tab[i,2],tab[i,3],tab[i,4]);
    pola[35]:=0;
    pola[i]:=2;
    brush.color:=clgray;
    ellipse(tab[35,1],tab[35,2],tab[35,3],tab[35,4]);
    end;
    end;
  end;

  if (pola[36]=3) then
  begin
    for i:=0 to 23 do begin
    if (pola[i]<>2) and (pola[i]<>1) and (x>tab[i,1]) and (y>tab[i,2]) and (x<tab[i,3]) and (y<tab[i,4]) then
    begin
    brush.color:=clblack;
    ellipse(tab[i,1],tab[i,2],tab[i,3],tab[i,4]);
    pola[36]:=0;
    pola[i]:=2;
    brush.color:=clgray;
    ellipse(tab[36,1],tab[36,2],tab[36,3],tab[36,4]);
    end;
    end;
  end;

  if (pola[37]=3) then
  begin
    for i:=0 to 23 do begin
    if (pola[i]<>2) and (pola[i]<>1) and (x>tab[i,1]) and (y>tab[i,2]) and (x<tab[i,3]) and (y<tab[i,4]) then
    begin
    brush.color:=clblack;
    ellipse(tab[i,1],tab[i,2],tab[i,3],tab[i,4]);
    pola[37]:=0;
    pola[i]:=2;
    brush.color:=clgray;
    ellipse(tab[37,1],tab[37,2],tab[37,3],tab[37,4]);
    end;
    end;
  end;

  if (pola[38]=3) then
  begin
    for i:=0 to 23 do begin
    if (pola[i]<>2) and (pola[i]<>1) and (x>tab[i,1]) and (y>tab[i,2]) and (x<tab[i,3]) and (y<tab[i,4]) then
    begin
    brush.color:=clblack;
    ellipse(tab[i,1],tab[i,2],tab[i,3],tab[i,4]);
    pola[38]:=0;
    pola[i]:=2;
    brush.color:=clgray;
    ellipse(tab[38,1],tab[38,2],tab[38,3],tab[38,4]);
    end;
    end;
  end;

  if (pola[39]=3) then
  begin
    for i:=0 to 23 do begin
    if (pola[i]<>2) and (pola[i]<>1) and (x>tab[i,1]) and (y>tab[i,2]) and (x<tab[i,3]) and (y<tab[i,4]) then
    begin
    brush.color:=clblack;
    ellipse(tab[i,1],tab[i,2],tab[i,3],tab[i,4]);
    pola[39]:=0;
    pola[i]:=2;
    brush.color:=clgray;
    ellipse(tab[39,1],tab[39,2],tab[39,3],tab[39,4]);
    end;
    end;
  end;

  if (pola[40]=3) then
  begin
    for i:=0 to 23 do begin
    if (pola[i]<>2) and (pola[i]<>1) and (x>tab[i,1]) and (y>tab[i,2]) and (x<tab[i,3]) and (y<tab[i,4]) then
    begin
    brush.color:=clblack;
    ellipse(tab[i,1],tab[i,2],tab[i,3],tab[i,4]);
    pola[40]:=0;
    pola[i]:=2;
    brush.color:=clgray;
    ellipse(tab[40,1],tab[40,2],tab[40,3],tab[40,4]);
    end;
    end;
  end;

  if (pola[41]=3) then
  begin
    for i:=0 to 23 do begin
    if (pola[i]<>2) and (pola[i]<>1) and (x>tab[i,1]) and (y>tab[i,2]) and (x<tab[i,3]) and (y<tab[i,4]) then
    begin
    brush.color:=clblack;
    ellipse(tab[i,1],tab[i,2],tab[i,3],tab[i,4]);
    pola[41]:=0;
    pola[i]:=2;
    brush.color:=clgray;
    ellipse(tab[41,1],tab[41,2],tab[41,3],tab[41,4]);
    end;
    end;
  end;


  end;

  end;
  end;


procedure TForm1.Shape1StartDrag(Sender: TObject; var DragObject: TDragObject);
begin
  begin
  with canvas do begin
    brush.color:=clblack;
    rectangle(100,100,300,300);
  end;
  end;

end;

end.


