# GITHUB-test
Learning file description
%GAUSS SEDIAL 
clc
clear all
sb=[1 1 2 4 3];                  %input('Enter the starting bus = ')
eb=[2 3 4 3 2];                  % input('Enter the ending bus = ')
nl=5;                            %input(' Enter the number of lines= ')
nb=4;                            %input(' Enter the number of buses= ')
sa=[1-5j 1.2-4j 1.1-2j 1.2-3j .5-4j]; %input('Enter the value of series impedance =')
Ybus=zeros(nb,nb);
for i=1:nl
    k1=sb(i);
    k2=eb(i) ;
    y(i)=sa(i);
    Ybus(k1,k1)=Ybus(k1,k1)+y(i);%+h(i);
    Ybus(k2,k2)=Ybus(k2,k2)+y(i);%+h(i);
    Ybus(k1,k2)=-y(i);
    Ybus(k2,k1)=Ybus(k1,k2);
end
Ybus
PG=[0 .5 .4 .2];
QG=[0 0 .3j .1j];
V=[1.06 1.04 1 1];
Qmin=.05;
Qmax=.12;
for i=1:nb
    Pg=PG(i);
    Qg=QG(i);
        if(Pg==0&&Qg==0)%for slackbus
        p=1;
        Vt(p)=V(p) ;
        end
        if(Pg~=0&&Qg==0)%for Generator bus
         for q=1:p-1
          A=Ybus(p,q)*V(q);
      end
      B=0;
      for q=p:nb
         B=B+Ybus(p,q)*V(q);
            end
            c=V(p)*(A+B);
            Q=-imag(c)
    if(Qmin<=Q&&Q<=Qmax)%check for Q limt
        Qg=Q*j;
        Vt(p)=V(p);
    else
        if(Q<=Qmin)
            Q=Qmin;
        else
            Q=Qmax;
        end
        Vt(p)=1;
        disp('it is load bus')
        Qg=Q*j
    end
     QG(p)=Qg;
         for q=1:p-1
          A1=Ybus(p,q)*V(q);
        end
        B1=0;
      for q=p+1:nb
         B1=B1-(((Ybus(p,q))*V(q)));
      end
      C1=((PG(p)-(QG(p)))/Vt(p));
      Vt(p)=((C1-A1+B1)/Ybus(p,p));
     elseif(Pg~=0&&Qg~=0)%for load bus
            A2=0;
            for q=1:p-1
          A2=A2-Ybus(p,q)*Vt(q);
        end
        B2=0;
      for q=p+1:nb
         B2=B2-(((Ybus(p,q))*V(q)));
      end
      C2=(-PG(p)+(QG(p))/V(p));
      Vt(p)=((C2+A2+B2)/Ybus(p,p))
        end
    p=p+1;
end

