# Vending-machine-Controller
This Vending machine controller is built from two sub-blocks

1)Positive edge triggered D flipflop

2)Positive edge triggered D flipflop with preset

One hot state representation is used for design of this controller

# Positive edge triggered D flipflop

module dff(
    
    input d,
    
    input clk,
    
    input reset,
    
    output reg q
    
    );
    
    always@(posedge clk,posedge reset)
    
    begin
    
        if (reset==1'b1)
        
            q<=0;
        
        else if(reset==1'b0)
        
            q<=d;
        
        end

endmodule
---------------------------------------------------
# Positive edge triggered D flipflop with preset
module dffp(
    
    input d,
    
    input clk,
    
    input preset,
    
    output reg q
    
    );
    
    always@(posedge clk,posedge preset)
    
    begin
    
        if (preset==1'b1)
        
            q<=1;
        
        else if(preset==1'b0)
        
            q<=d;
    
    end

endmodule
--------------------------------------------
# The Vending machine Controller

module vendingmachine(
    
    input clk,
    
    input reset,
    
    input ca,
    
    input s2,
    
    input s1,
    
    input s0,
    
    output cca,
    
    output ready,
    
    output p1,
    
    output p2,
    
    output p3,
    
    output p4,
    
    output p5
    
    );
    
    wire a,b,c,d,e,f,ap,bp,cp,dp,ep,fp;
    
    dffp ff1(ap,clk,reset,a);
    
    dff ff2(bp,clk,reset,b);
    
    dff ff3(cp,clk,reset,c);
    
    dff ff4(dp,clk,reset,d);
    
    dff ff5(ep,clk,reset,e);
    
    dff ff6(fp,clk,reset,f);
    
    assign ready = (a & ~ca) |( s2&s1&~s0 )|(s2 & s1 & s0) | b |c | d| e | f;
    
    assign cca= ~ca | b | c | d | e | f;
    
    assign ap=(a& ~ca) | b | c | d | e | f;
    
    assign bp=( a & ca & ~s2 & ~s1 & s0);
    
    assign p1=bp;
    
    assign cp=( a & ca & ~s2 & s1 & ~s0);
    
    assign p2=cp;
    
    assign dp=( a & ca & ~s2 & s1 & s0);
    
    assign p3=dp;
    
    assign ep=( a & ca & s2 & ~s1 & ~s0);
    
    assign p4=ep;
    
    assign fp=( a & ca & s2 & ~s1 & s0);
    
    assign p5=fp;

endmodule

--------------------------------------
# Test bench

module vending_tb(

    );
   
    reg clk,reset,ca,s2,s1,s0;
    
    wire cca,ready,p1,p2,p3,p4,p5;
    
    vendingmachine uut(clk,reset,ca,s2,s1,s0,cca,ready,p1,p2,p3,p4,p5);
    
    initial begin
    
        clk=1'b1;
        
        forever #5 clk=~clk;
     
     end
     
     initial begin
     
        reset=1'b1;
        
        ca=0;
        
        s2=0;   s1=0;   s0=0;
        
        #5;
        
        reset=1'b0;
        
        #20;
        
         ca=1;
         
         s2=0;   s1=0;   s0=1;
        
        #5;
        
        ca=0;
        
        #15;
        
        ca=1;
        
        s2=0;   s1=1;   s0=0;
        
        #5;
        
        ca=0;
        
        #15;
        
        ca=1;
        
        s2=0;   s1=1;   s0=1;
        
        #5;
        
        ca=0;
        
        #15;
        
        ca=1;
        
        s2=1;   s1=0;   s0=0;
        
        #5;
        
        ca=0;
        
        #15
        
        ca=1;
        
        s2=1;   s1=0;   s0=1;
        
        #5;
        
        ca=0;
        
        #20 $finish;


     end

endmodule
--------------------------------------
# The Simulation Output

![vending](https://github.com/vishveshgoud/Vending-machine-Controller/assets/147975068/4429ef01-1035-4235-a60e-566d3196c234)
