# Cutscene Asset Loader (Retail)  
All cutscene DEFFs have some asset loading code and an asset table, even if they are unused  
  
## Asset Table  
All asset data entries are in groups of 4 (rows r0, r1, r2, r3), and what they load depends on their structure rather than the caller  
If r1 and r2 are 0, that entry is used to load a texture to vram, where r3 is DRGN0 file index  
If r2 is 0 but r1 is not, then that entry loads an animation into the combatant **at var[45][r0]**, at asset index r1, where r3 is the DRGN0 file (animation) to load  
If all entries are used, this will load a new battle entity, where:
- r0 is **var[45] index that a combatant gets created into (so 0x4 means that this combatant index will be accessible via var[45][4])** ***ITS NOT COMBATANT INDEX, ITS VAR45 INDEX!***   
- r1 is **var[45] index for newly created script (so 0x10 means that this script will be accessible via var[45][16]), this number is **always** plus 10 of the combatant var45 index  
- r2 is DRGN0 animation file for slot 0
- r3 is DRGN0 TMD file  

Battle entity loader will always load texture for its TMD.  


### Example Asset Table
0x0;     First Entry  
0x0;     Unused  
0x0;     Unused    
0x272;   Second Entry, loads DRGN file 0x272 (some texture) into VRAM  
  
0x2;     Second Entry, var[45][2] is where newly created combatant index will be stored  
0xc;     var[45][12] is where newly created script for combatant will be stored  
0x252;   DRGN0 file 0x252 (animation) will be loaded and inserted into asset slot 0  
0x250;   DRGN0 file 0x250 (TMD file) will be loaded for this combatant  
  
0x2;     Third Entry, load some animation into script at var[45][2]   
0x1;     Stands for first slot  
0x0;     Unused  
0x253;   DRGN0 file 0x253 (animation) will be loaded into slot 1 of script at var[45][2]  