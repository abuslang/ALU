//	Author: Abdus Quadri


//16->1 Mux
// ======================================================================================================
module Multiplexer(Ch14,Ch13,Ch12,Ch11,Ch10,Ch9,Ch8,Ch7,Ch6,Ch5,Ch4,Ch3,Ch2, Ch1, Ch0, s, b) ;
  parameter bus_size = 32;
  input [bus_size-1:0] Ch14,Ch13,Ch12,Ch11,Ch10,Ch9,Ch8,Ch7,Ch6,Ch5,Ch4,Ch3,Ch2, Ch1, Ch0 ;  // inputs
  input [15:0] s ; // one-hot select
  output[bus_size-1:0] b ;
  assign b = 
  ({bus_size{s[14]}} & Ch14) | 
  ({bus_size{s[13]}} & Ch13) | 
  ({bus_size{s[12]}} & Ch12) |   
  ({bus_size{s[11]}} & Ch11) |   
  ({bus_size{s[10]}} & Ch10) | 
  ({bus_size{s[9]}} & Ch9) |
  ({bus_size{s[8]}} & Ch8) |
  ({bus_size{s[7]}} & Ch7) | 
  ({bus_size{s[6]}} & Ch6) | 
  ({bus_size{s[5]}} & Ch5) | 
  ({bus_size{s[4]}} & Ch4) |   
  ({bus_size{s[3]}} & Ch3) |   
  ({bus_size{s[2]}} & Ch2) | 
  ({bus_size{s[1]}} & Ch1) |
  ({bus_size{s[0]}} & Ch0) ;
endmodule


// =======================================================================================================
//4->16 decoder
// =======================================================================================================
module decoder(a, b);
  parameter n = 4;
  parameter m = 16;
  
  input [n-1:0] a;
  output [m-1:0] b;
  wire [m-1: 0] b = 1 << a;
  
endmodule

// =======================================================================================================
// 16 bit Multiplier
// =======================================================================================================
module Multiplier16bit(a,b,plow,phigh);
parameter size= 16;

input [size-1:0] a;
input [size-1:0] b;


output [size-1:0] plow;
output [size-1:0] phigh;
//output [31:0] temp;

wire [size-1:0] pp0 = a & {size{b[0]}};
wire [size-1:0] pp1 = a & {size{b[1]}};
wire [size-1:0] pp2 = a & {size{b[2]}};
wire [size-1:0] pp3 = a & {size{b[3]}};
wire [size-1:0] pp4 = a & {size{b[4]}};
wire [size-1:0] pp5 = a & {size{b[5]}};
wire [size-1:0] pp6 = a & {size{b[6]}};
wire [size-1:0] pp7 = a & {size{b[7]}};
wire [size-1:0] pp8 = a & {size{b[8]}};
wire [size-1:0] pp9 = a & {size{b[9]}};
wire [size-1:0] pp10 = a & {size{b[10]}};
wire [size-1:0] pp11 = a & {size{b[11]}};
wire [size-1:0] pp12 = a & {size{b[12]}};
wire [size-1:0] pp13 = a & {size{b[13]}};
wire [size-1:0] pp14 = a & {size{b[14]}};
wire [size-1:0] pp15 = a & {size{b[15]}};



wire [size-1:0] car;
wire [size-1:0] s1,s2,s3,s4,s5,s6,s7,s8,s9,s10,s11,s12,s13,s14,s15;

FullAdder16Bit a1(pp1,{1'b0,pp0[size-1:1]},1'b0,s1,car[0]);
FullAdder16Bit a2(pp2,{car[0],s1[size-1:1]},1'b0,s2,car[1]);
FullAdder16Bit a3(pp3,{car[1],s2[size-1:1]},1'b0,s3,car[2]);
FullAdder16Bit a4(pp4,{car[2],s3[size-1:1]},1'b0,s4,car[3]);
FullAdder16Bit a5(pp5,{car[3],s4[size-1:1]},1'b0,s5,car[4]);
FullAdder16Bit a6(pp6,{car[4],s5[size-1:1]},1'b0,s6,car[5]);
FullAdder16Bit a7(pp7,{car[5],s6[size-1:1]},1'b0,s7,car[6]);
FullAdder16Bit a8(pp8,{car[6],s7[size-1:1]},1'b0,s8,car[7]);

FullAdder16Bit a9(pp9,{car[7],s8[size-1:1]},1'b0,s9,car[8]);
FullAdder16Bit a10(pp10,{car[8],s9[size-1:1]},1'b0,s10,car[9]);
FullAdder16Bit a11(pp11,{car[9],s10[size-1:1]},1'b0,s11,car[10]);
FullAdder16Bit a12(pp12,{car[10],s11[size-1:1]},1'b0,s12,car[11]);
FullAdder16Bit a13(pp13,{car[11],s12[size-1:1]},1'b0,s13,car[12]);
FullAdder16Bit a14(pp14,{car[12],s13[size-1:1]},1'b0,s14,car[13]);
FullAdder16Bit a15(pp15,{car[13],s14[size-1:1]},1'b0,s15,car[14]);

assign phigh = {car[14],s15[size-1:1] };
assign plow = {s15[0],s14[0],s13[0],s12[0],s11[0],s10[0],s9[0],s8[0],s7[0],s6[0],s5[0],s4[0],s3[0],s2[0],s1[0],pp0[0]};

endmodule

// =======================================================================================================
// 16 bit divider
// =======================================================================================================
module Divider16bit(a,b,q,r);
input [15:0] a;
input [15:0] b;
output [15:0] q;
output [15:0] r;
reg [15:0] w;
reg [15:0] g;
always @(a,b,q,r)
begin
w = a/b;
g = a%b;
end

assign q = w;
assign r = g; 
endmodule
// =========================================================================================================


//================================
// Proper Half Adder   ####taken from fun.v
//================================  
module HalfAdder1Bit(a,b,c,s) ;
  input a,b ;
  output c,s ;  // carry and sum
  assign s = a ^ b ;
  assign c = a & b ;
endmodule

//----------------------------------------------------------------------
// full adder - from half adders  ####taken from fun.v
module FullAdder1Bit(a,b,cin,cout,s) ;
  input a,b,cin ;
  output cout,s ;  // carry and sum
  wire g,p ;	   // generate and propagate
  wire cp ;
  HalfAdder1Bit ha1(a,b,g,p) ;
  HalfAdder1Bit ha2(cin,p,cp,s) ;
  assign cout = g | cp;
endmodule


//================================
// FULL ADDER (4 BIT)	####taken from fun.v
//================================
module FullAdder4Bit(a,b,cin,s,cout);
parameter size=4;
input [size-1:0] a;
input [size-1:0] b;
input cin;

output [size-1:0] s;
output cout;

wire  sum[size-1:0];
wire  car[size-1:0];

FullAdder1Bit fa0(a[0],b[0],cin,car[0],sum[0]);
FullAdder1Bit fa1(a[1],b[1],car[0],car[1],sum[1]);
FullAdder1Bit fa2(a[2],b[2],car[1],car[2],sum[2]);
FullAdder1Bit fa3(a[3],b[3],car[2],car[3],sum[3]);

assign cout=car[3];
assign s[0]=sum[0];
assign s[1]=sum[1];
assign s[2]=sum[2];
assign s[3]=sum[3];


endmodule


//================================
// FULL ADDER  (16 BIT)	####taken and modified from fun.v
//================================
module FullAdder16Bit(a,b,cin,s,cout);
parameter size= 16;
input [size-1:0] a;
input [size-1:0] b;
input cin;

output [size-1:0] s;
output cout;

wire  sum[size-1:0];
wire  car[size-1:0];

FullAdder4Bit f4a0(a[3:0],b[3:0],cin,s[3:0],car[0]);
FullAdder4Bit f4a1(a[7:4],b[7:4],car[0],s[7:4],car[1]);
FullAdder4Bit f4a2(a[11:8],b[11:8],car[1],s[11:8],car[2]);


FullAdder4Bit f4a3(a[15:12],b[15:12],car[2],s[15:12],car[3]);


assign cout=car[3];
assign s[0]=sum[0];
assign s[1]=sum[1];
assign s[2]=sum[2];
assign s[3]=sum[3];
assign s[4]=sum[4];
assign s[5]=sum[5];
assign s[6]=sum[6];
assign s[7]=sum[7];
assign s[8]=sum[8];
assign s[9]=sum[9];
assign s[10]=sum[10];
assign s[11]=sum[11];
assign s[12]=sum[12];
assign s[13]=sum[13];
assign s[14]=sum[14];
assign s[15]=sum[15];


endmodule


//
// FULL ADDER with overflow data (16 BIT)	####taken and modified from fun.v
//================================
module FullAdder16(a,b,cin,s,cout,over);
parameter size= 16;
input [size-1:0] a;
input [size-1:0] b;
input cin;

output [size-1:0] s;
output cout;
output over;

wire  sum[size-1:0];
wire  car[size-1:0];
wire flow;

FullAdder4Bit f4a0(a[3:0],b[3:0],cin,s[3:0],car[0]);
FullAdder4Bit f4a1(a[7:4],b[7:4],car[0],s[7:4],car[1]);
FullAdder4Bit f4a2(a[11:8],b[11:8],car[1],s[11:8],car[2]);

//last must be seperated to allow for over flow info

FullAdder1Bit fa0(a[12],b[12],car[2],car[3],s[12]);
FullAdder1Bit fa1(a[13],b[13],car[3],car[4],s[13]);
FullAdder1Bit fa2(a[14],b[14],car[4],car[5],sum[14]);
FullAdder1Bit fa3(a[15],b[15],car[5],car[6],sum[15]);
//FullAdder4Bit f4a3(a[15:12],b[15:12],car[2],s[15:12],car[3]);


assign over = car[6]^car[5];
assign cout=car[6];
assign s[0]=sum[0];
assign s[1]=sum[1];
assign s[2]=sum[2];
assign s[3]=sum[3];
assign s[4]=sum[4];
assign s[5]=sum[5];
assign s[6]=sum[6];
assign s[7]=sum[7];
assign s[8]=sum[8];
assign s[9]=sum[9];
assign s[10]=sum[10];
assign s[11]=sum[11];
assign s[12]=sum[12];
assign s[13]=sum[13];
assign s[14]=sum[14];
assign s[15]=sum[15];


// =============================================================================================
endmodule
module Adder_Subtractor(a,b,cin,s,cout,overflow);
parameter size= 16;
input [size-1:0] a;
input [size-1:0] b;
input cin;

output [size-1:0] s;
output cout;
output overflow;
wire [size-1:0] out;
Gate_XOR xr(b,{(size){cin}},out);
FullAdder16 fn(a,out,cin,s,cout,overflow);


endmodule
//===============================================================================================

module Gate_NOT(a,r); //##modified and taken from fun.v
parameter size=16;
input [size-1:0] a;
input [size-1:0] b;
output [31:0] r;
//--------------------
assign r=~a;
//--------------------
endmodule

module Gate_NOR(a,b,r);
parameter size=16;
input [size-1:0] a;
input [size-1:0] b;
output [31:0] r;
assign r =  ~(a | b);
endmodule


//================================##modified and taken from fun.v
module Gate_XOR(a,b,r);
parameter size=16;
input [size-1:0] a;
input [size-1:0] b;
output [31:0] r;
//--------------------
assign r=a^b;
//--------------------
endmodule

//=========================================================================================================

module Gate_AND(a,b,r);
parameter size=16;
input [size-1:0] a;
input [size-1:0] b;
output [31:0] r;
//--------------------
assign r=a&&b;
//--------------------
endmodule

//=========================================================================================================
module Gate_NAND(a,b,r);
parameter size=16;
input [size-1:0] a;
input [size-1:0] b;
output [31:0] r;
//--------------------
assign r=!(a&&b);
//--------------------
endmodule

//=========================================================================================================
module Gate_XNOR(a,b,r);
parameter size=16;
input [size-1:0] a;
input [size-1:0] b;
output [31:0] r;
assign r =  ~(a ^ b);
endmodule

//=========================================================================================================
module Gate_OR(a,b,r);
parameter size=16;
input [size-1:0] a;
input [size-1:0] b;
output [31:0] r;
assign r = a|b;
endmodule

//=========================================================================================================
module reg_16bit(clk, d, q, r);
  input clk, r;
  input [15: 0]d;
  output[15: 0]q;
  reg[15: 0]q;
  
  always @(posedge clk or posedge r)
    begin
      if(r)
        q <= 16'b0000000000000000;
      else if(clk)
        q <= d;
    end
endmodule

//=========================================================================================================
module reg_32bit(clk, d, q, r);
  input clk, r;
  input [31: 0]d;
  output[31: 0]q;
  reg[31: 0]q;
  
  always @(posedge clk or posedge r)
    begin
      if(r)
        q <= 32'b00000000000000000000000000000000;
      else if(clk)
        q <= d;
    end
endmodule


//=========================================================================================================
module alu(A, B, opcode, r, clk);

	parameter size = 31;
	
	input [15:0] A, B;
	input [3:0] opcode;
	input r, clk;
	input [size:0] out;
	
	
	
	//for Multiplexer
	wire [size:0] Ch15,Ch14,Ch13,Ch12,Ch11,Ch10,Ch9,Ch8,Ch7,Ch6,Ch5,Ch4,Ch3,Ch2,Ch1,Ch0 ;  // inputs
	wire [size:0] channelOut ;
	// for decoder
	wire [15:0] sout;
	// for adder
	wire [15:0] sum;
	wire cin, cout;
	wire cin3, cout3;
	// for subtractoor
	wire [15:0] difference;
	wire cin2, cout2;
	// for multiplier
	wire [15:0] plow, phigh;
	
	wire [31:0] product;
	assign product = {phigh, plow};
	
	// for divider / modulus
	wire [15:0] quotient, remainder;
	wire [31:0] fullQuotient;
	assign fullQuotient = {quotient, remainder};
	// and
	wire [size:0] andResult;
	// or
	wire [size:0] orResult;
	// not
	wire [size:0] notResult;
	// xor
	wire [size:0] xorResult;
	// nand
	wire [size:0] nandResult;
	// nor
	wire [size:0] norResult;
	// xnor
	wire [size:0] xnorResult;
	

	// -------------------------------------
	
	Adder_Subtractor adder(A, B, 1'b0, sum, cout, overflow);
	Adder_Subtractor subtractor(A, B, 1'b1, difference, cout2, overflow2);
	
	Multiplier16bit multiply(A, B, plow, phigh);
	Divider16bit divide(A, B, quotient, remainder);
	
	Gate_AND and_ab(A, B, andResult);
	Gate_OR or_ab(A, B, orResult);
	Gate_NOT not_a(A, notResult);
	Gate_XOR xor_ab(A, B, xorResult);
	Gate_NAND nand_ab(A, B, nandResult);
	Gate_NOR nor_ab(A, B, norResult);
	Gate_XNOR xnor_ab(A, B, xnorResult);

	decoder dec(opcode, sout);
		
	
	Multiplexer mux(A,B,xnorResult,norResult,nandResult,xnorResult,notResult,orResult,andResult,remainder,fullQuotient,product,difference,sum,out, sout, channelOut);

	
	Adder_Subtractor outputReg(channelOut, 0, 1'b0, out, cout3, overflow3);
	//out = channelOut;

	
endmodule
	
	

module testbench();

reg [15:0] A, B;
//reg [31:0] out;
reg [3:0] opcode;
reg clk, r, err;

reg [4:0] i;
reg [4:0] j;
reg [31:0] tmp;
		
alu calc(A, B, opcode, r, clk);

//---------------------------------------------
//The Display Thread with Clock Control
//---------------------------------------------
   initial
    begin
	  forever
			begin
					#5 
					clk = 0 ;
					#5
					clk = 1 ;
			end
   end	


initial begin
	
		
		A = 6990;
		B = 3201;
		opcode = 0;
		r = 0;
		
	    $display("++================++================++======++===++===++===================================++");
		$display("||       A        ||       B        ||Opcode||Clk||Err||\t\tOutput\t\t   ||");
		
		
		err = 0;
		tmp = calc.mux.b;
		
		//A for loop, with register i being the loop control variable.
		for (i = 0; i < 16; i = i + 1) 
		 //always@(*)
		 begin//Open the code blook of the for loop
			for (j = 0; j < 2; j = j + 1) 
				begin
		
			
				

				#5;
				


				if(clk == 1)
					tmp = calc.mux.b;
				//$display("tmp: %b", tmp);
				
				if(calc.adder.overflow == 1 || calc.subtractor.overflow == 1)
					err = 1;
				
				if(B == 0 && opcode == 4'b0100)
					err = 1;
					
				
				if( opcode == 4'b1101)
					A = tmp;
				if( opcode == 4'b1110)
					begin
						A = 0;
						B = 0;
						tmp = 0;
						err = 0;
						opcode = 1;
						//#5;
						opcode = 4'b1110;
					end
				$display ("  %b  %b  %b    %b    %b    %b", A, B, opcode, clk, err, tmp);
				if(i%4==3) //Every fourth row of the table, put in a marker for easier reading.
				 $display ("++================++================++======++===++===++===================================++");
				 opcode = i;
				 
				 if(err == 1)
					opcode = 4'b1110;
		  
			end
		end//End of the for loop code block
	 
		#10 //A time de/4y of 10 time units. Hashtag Delay
		$finish;//A command, not unlike System.exit(0) in Java.
		
	end
	
endmodule






