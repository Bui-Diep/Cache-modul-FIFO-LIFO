-- FIFO cache module
library ieee;
use ieee.std_logic_1164.all;

entity fifo_cache is
  generic (
    width: integer;  -- width of data stored in cache
    depth: integer   -- depth of cache (number of items it can store)
  );
  port (
    clk: in std_logic;          -- clock signal
    rst: in std_logic;          -- reset signal
    din: in std_logic_vector;   -- data input
    wr_en: in std_logic;        -- write enable
    dout: out std_logic_vector; -- data output
    rd_en: in std_logic;        -- read enable
    full: out std_logic;        -- full flag
    empty: out std_logic        -- empty flag
  );
end fifo_cache;

architecture rtl of fifo_cache is
  type cache_type is array (0 to depth-1) of std_logic_vector(width-1 downto 0);
  signal cache: cache_type;    -- cache memory
  signal wr_ptr: integer range 0 to depth-1;  -- write pointer
  signal rd_ptr: integer range 0 to depth-1;  -- read pointer

begin
  -- Write operation
  process(clk, rst)
  begin
    if rst='1' then
      wr_ptr <= 0;
    elsif rising_edge(clk) then
      if wr_en='1' then
        cache(wr_ptr) <= din;
        wr_ptr <= wr_ptr + 1;
        if wr_ptr = depth then
          wr_ptr <= 0;
        end if;
      end if;
    end if;
  end process;

  -- Read operation
  process(clk, rst)
  begin
    if rst='1' then
      rd_ptr <= 0;
    elsif rising_edge(clk) then
      if rd_en='1' then
        dout <= cache(rd_ptr);
        rd_ptr <= rd_ptr + 1;
        if rd_ptr = depth then
          rd_ptr <= 0;
        end if;
      end if;
    end if;
  end process;

  -- Full flag
  full <= '1' when wr_ptr = rd_ptr and wr_en = '1' else '0';

  -- Empty flag
  empty <= '1' when wr_ptr = rd_ptr and rd_en = '1' else '0';

end rtl;
