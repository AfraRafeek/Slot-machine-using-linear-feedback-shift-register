module slot_machine(
    input clk,
    input reset,
    input spin,
    output reg [2:0] reel1,
    output reg [2:0] reel2,
    output reg [2:0] reel3,
    output reg jackpot
);

reg [7:0] lfsr;

always @(posedge clk or posedge reset)
begin
    if (reset)
    begin
        lfsr <= 8'b10101101;
        reel1 <= 0;
        reel2 <= 0;
        reel3 <= 0;
        jackpot <= 0;
    end
    else if (spin)
    begin
        lfsr <= {lfsr[6:0],
                 lfsr[7]^lfsr[5]^lfsr[4]^lfsr[3]};

        reel1 <= lfsr[2:0];
        reel2 <= lfsr[5:3];
        reel3 <= {lfsr[7], lfsr[1:0]};

        if ((lfsr[2:0] == lfsr[5:3]) &&
            (lfsr[5:3] == {lfsr[7], lfsr[1:0]}))
            jackpot <= 1;
        else
            jackpot <= 0;
    end
end

endmodule
