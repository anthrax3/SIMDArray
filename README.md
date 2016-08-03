# SIMDArray FSharp
SIMD enhanced Array operations for F#

## Example Usage

``` F#
open SIMDArray

//Faster map
let array = [| 1 .. 1000 |]
let squaredArray = array |> Array.SIMD.map (fun x -> x*x)

//Faster create and sum
let newArray = Array.SIMD.create 1000 5 //create a new array of length 1000 filled with 5
let sum = Array.SIMD.sum newArray

```

## Notes

Only 64 bit builds are supported.  Performance improvements will vary depending on your CPU architecture, width of Vector type, and the operations
you apply.  For small arrays the core libs may be faster due to increased fixed overhead for SIMD versions of the operations. To test
performance be sure to use Release builds with optimizations turned on.

## Performance Comparison vs Standrd Array Functions
With 32bit Floats
```ini

Host Process Environment Information:
BenchmarkDotNet=v0.9.8.0
OS=Microsoft Windows NT 6.2.9200.0
Processor=Intel(R) Core(TM) i7-4712HQ CPU 2.30GHz, ProcessorCount=8
Frequency=2240907 ticks, Resolution=446.2479 ns, Timer=TSC
CLR=MS.NET 4.0.30319.42000, Arch=64-bit RELEASE [RyuJIT]
GC=Concurrent Workstation
JitModules=clrjit-v4.6.1590.0

Type=SIMDBenchmark  Mode=Throughput  Platform=X64  
Jit=RyuJit  GarbageCollection=Concurrent Workstation  

```
       Method |  Length |            Median |         StdDev | Gen 0 | Gen 1 |  Gen 2 | Bytes Allocated/Op |
------------- |-------- |------------------ |--------------- |------ |------ |------- |------------------- |
 **SIMDContains** |      **10** |        **32.3354 ns** |      **0.0933 ns** |  **0.04** |     **-** |      **-** |              **22.80** |
     Contains |      10 |        13.0234 ns |      0.6457 ns |     - |     - |      - |               0.00 |
      SIMDMap |      10 |        37.3615 ns |      0.0693 ns |  0.09 |     - |      - |              53.95 |
          Map |      10 |        15.6651 ns |      0.2422 ns |  0.04 |     - |      - |              25.80 |
      SIMDSum |      10 |        19.3450 ns |      0.1866 ns |     - |     - |      - |               0.00 |
          Sum |      10 |         6.2273 ns |      0.2982 ns |     - |     - |      - |               0.00 |
      SIMDMax |      10 |        20.8972 ns |      0.7380 ns |     - |     - |      - |               0.00 |
          Max |      10 |         7.9275 ns |      0.9701 ns |     - |     - |      - |               0.00 |
 **SIMDContains** |     **100** |        **61.6295 ns** |      **5.0472 ns** |  **0.04** |     **-** |      **-** |              **24.92** |
     Contains |     100 |       140.9920 ns |      2.4739 ns |     - |     - |      - |               0.01 |
      SIMDMap |     100 |        75.8733 ns |      0.5875 ns |  0.33 |     - |      - |             192.40 |
          Map |     100 |       120.3029 ns |      0.4232 ns |  0.29 |     - |      - |             172.39 |
      SIMDSum |     100 |        32.0058 ns |      1.1225 ns |     - |     - |      - |               0.00 |
          Sum |     100 |        77.6100 ns |      2.4902 ns |     - |     - |      - |               0.00 |
      SIMDMax |     100 |        35.9042 ns |      2.0587 ns |     - |     - |      - |               0.00 |
          Max |     100 |        92.1754 ns |      9.6637 ns |     - |     - |      - |               0.00 |
 **SIMDContains** |    **1000** |       **417.0760 ns** |     **10.6672 ns** |     **-** |     **-** |      **-** |               **0.04** |
     Contains |    1000 |     1,333.0239 ns |     11.8959 ns |     - |     - |      - |               0.07 |
      SIMDMap |    1000 |       439.8549 ns |      7.5810 ns |  3.05 |     - |      - |           2,176.91 |
          Map |    1000 |     1,073.2894 ns |     16.1444 ns |  2.93 |     - |      - |           2,086.24 |
      SIMDSum |    1000 |       162.8308 ns |      5.8158 ns |     - |     - |      - |               0.01 |
          Sum |    1000 |       947.1124 ns |     14.4370 ns |     - |     - |      - |               0.07 |
      SIMDMax |    1000 |       167.0257 ns |      5.3584 ns |     - |     - |      - |               0.01 |
          Max |    1000 |       698.2252 ns |     21.2244 ns |     - |     - |      - |               0.03 |
 **SIMDContains** | **1000000** |   **427,765.2001 ns** |  **3,541.8344 ns** |     **-** |     **-** |   **0.23** |           **7,507.17** |
     Contains | 1000000 | 1,315,198.8375 ns | 19,634.6409 ns |     - |     - |   0.36 |          14,912.24 |
      SIMDMap | 1000000 | 1,747,002.9295 ns | 18,219.0807 ns |     - |     - | 519.18 |       1,198,305.57 |
          Map | 1000000 | 1,962,408.1761 ns | 23,319.8186 ns |     - |     - | 746.00 |       1,702,687.72 |
      SIMDSum | 1000000 |   160,972.7015 ns |  3,359.1696 ns |     - |     - |   0.05 |           1,960.97 |
          Sum | 1000000 |   955,224.0942 ns | 12,365.7613 ns |     - |     - |   0.38 |          14,853.87 |
      SIMDMax | 1000000 |   158,835.3746 ns |  3,773.1697 ns |     - |     - |   0.06 |           1,961.66 |
          Max | 1000000 |   633,761.7634 ns |  6,149.8767 ns |     - |     - |   0.24 |           7,495.76 |

