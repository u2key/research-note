# HPS Communication with FPGA

## Sample C Code

```
#include <stdio.h>
#include <stdint.h>
#include <stdlib.h>
#include <string.h>
#include <fcntl.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/mman.h>
#include "hps_addr.h"

#define ADDR_BASE 0xFF200000
#define ADDR_SPAN 0x00200000
#define BUF_SIZE  256
#define HEX_0     0x40
#define HEX_1     0x79
#define HEX_2     0x24
#define HEX_3     0x30
#define HEX_4     0x19
#define HEX_5     0x12
#define HEX_6     0x02
#define HEX_7     0x78
#define HEX_8     0x00
#define HEX_9     0x10
#define HEX_A     0x08
#define HEX_B     0x03
#define HEX_C     0x46
#define HEX_D     0x21
#define HEX_E     0x06
#define HEX_F     0x0E

extern int   open_phys();
extern void  close_phys();
extern void *map_phys();
extern int   unmap_phys();

struct list {
  uint32_t bin;
  char str[33];
  char hex[8];
  struct list *next;
};
typedef struct list list;
typedef struct timeval timeval;

int getVAddr(int fdsc, void **V_BASE, void **SW, void **HEX0, void **HEX1, void **HEX2, void **HEX3, void **HEX4, void **HEX5){
  if((fdsc = open_phys(fdsc)) == -1){
    return 1;
  }
  if(!(*V_BASE = map_phys(fdsc, ADDR_BASE, ADDR_SPAN))){
    return 1;
  }
  *SW   = (void *)(*V_BASE +   SW_BASE);
  *HEX0 = (void *)(*V_BASE + HEX0_BASE);
  *HEX1 = (void *)(*V_BASE + HEX1_BASE);
  *HEX2 = (void *)(*V_BASE + HEX2_BASE);
  *HEX3 = (void *)(*V_BASE + HEX3_BASE);
  *HEX4 = (void *)(*V_BASE + HEX4_BASE);
  *HEX5 = (void *)(*V_BASE + HEX5_BASE);
  return 0;
}

void setSelect(fd_set *fdsc_slct){
  FD_ZERO(fdsc_slct);
  FD_SET(0, fdsc_slct);
}

int isSameStr(char *str1, char *str2, int len){
  return strncmp(str1, str2, len) == 0;
}

void addNewTest(list **first_test, char *str){
  int i, j, len;
  list *final_test = *first_test;
  if(*first_test == NULL){
    final_test = *first_test = (list *)malloc(sizeof(list));
  }else{
    while(final_test->next != NULL){
      final_test = final_test->next;
    }
    final_test = final_test->next = (list *)malloc(sizeof(list));
  }
  len = strlen(str);
  for(i = 0; i < 32 - len; i++){
    final_test->str[i] = '0';
  }
  for(i = 0; i < len; i++){
    if(str[i] == '0' || str[i] == '1'){
      final_test->str[32 - len + i] = str[i];
    }else{
      final_test->str[32 - len + i] = '0';
    }
  }
  memset(&final_test->bin, 0, 4);
  for(i = 0; i < 32; i++){
    final_test->bin <<= 1;
    if(final_test->str[i] == '1'){
      final_test->bin |= 0x1;
    }
  }
  for(i = 0; i < 4; i++){
    j = 2 * i;
    switch(((char *)(&final_test->bin))[i] & 0xF){
      case  0: final_test->hex[j] = HEX_0; break;
      case  1: final_test->hex[j] = HEX_1; break;
      case  2: final_test->hex[j] = HEX_2; break;
      case  3: final_test->hex[j] = HEX_3; break;
      case  4: final_test->hex[j] = HEX_4; break;
      case  5: final_test->hex[j] = HEX_5; break;
      case  6: final_test->hex[j] = HEX_6; break;
      case  7: final_test->hex[j] = HEX_7; break;
      case  8: final_test->hex[j] = HEX_8; break;
      case  9: final_test->hex[j] = HEX_9; break;
      case 10: final_test->hex[j] = HEX_A; break;
      case 11: final_test->hex[j] = HEX_B; break;
      case 12: final_test->hex[j] = HEX_C; break;
      case 13: final_test->hex[j] = HEX_D; break;
      case 14: final_test->hex[j] = HEX_E; break;
      case 15: final_test->hex[j] = HEX_F; break;
    }
    j = 2 * i + 1;
    switch(((char *)(&final_test->bin))[i] >> 4){
      case  0: final_test->hex[j] = HEX_0; break;
      case  1: final_test->hex[j] = HEX_1; break;
      case  2: final_test->hex[j] = HEX_2; break;
      case  3: final_test->hex[j] = HEX_3; break;
      case  4: final_test->hex[j] = HEX_4; break;
      case  5: final_test->hex[j] = HEX_5; break;
      case  6: final_test->hex[j] = HEX_6; break;
      case  7: final_test->hex[j] = HEX_7; break;
      case  8: final_test->hex[j] = HEX_8; break;
      case  9: final_test->hex[j] = HEX_9; break;
      case 10: final_test->hex[j] = HEX_A; break;
      case 11: final_test->hex[j] = HEX_B; break;
      case 12: final_test->hex[j] = HEX_C; break;
      case 13: final_test->hex[j] = HEX_D; break;
      case 14: final_test->hex[j] = HEX_E; break;
      case 15: final_test->hex[j] = HEX_F; break;
    }
  }
}

int next_test(list **cur_test){
  if((*cur_test)->next == NULL){
    return 1;
  }else{
    *cur_test = (*cur_test)->next;
  }
  return 0;
}

void freeVAddr(void *V_BASE, int fdsc){
  unmap_phys(V_BASE, ADDR_SPAN);
  close_phys(fdsc);
}

int main(int args, char *argv[]){
  void *V_BASE, *SW, *HEX0, *HEX1, *HEX2, *HEX3, *HEX4, *HEX5;
  int fdsc = -1, test_run = 1, slct, len;
  FILE *ftest;
  char buf[BUF_SIZE], exec[BUF_SIZE];
  list *cur_test = NULL;
  fd_set fdsc_slct, fdsc_read;
  timeval timeout = {1, 0};
  setvbuf(stdout, NULL, _IONBF, 0);
  if(getVAddr(fdsc, &V_BASE, &SW, &HEX0, &HEX1, &HEX2, &HEX3, &HEX4, &HEX5)){
    return 1;
  }
  setSelect(&fdsc_slct);
  if(args < 2){
    printf("Input: ");
    buf[read(0, buf, BUF_SIZE) - 1] = '\0';
    addNewTest(&cur_test, buf);
  }else{
    if((ftest = fopen(argv[1], "r")) == NULL){
      printf("Failed to Open the File\n");
      return 1;
    }
    while(fgets(exec, BUF_SIZE, ftest) != NULL){
      len = strlen(exec);
      if(exec[len - 1] == '\n'){
        exec[len - 1] = '\0';
      }
      addNewTest(&cur_test, exec);
    }
    fclose(ftest);
  }
  while(test_run){
    memcpy(&fdsc_read, &fdsc_slct, sizeof(fdsc_slct));
    slct = select(1, &fdsc_read, NULL, NULL, &timeout);
    printf("EXEC: %s\n", cur_test->str);
    if(*(uint32_t *)SW == 0){
      *(char *)HEX0 = cur_test->hex[0];
      *(char *)HEX1 = cur_test->hex[1];
      *(char *)HEX2 = cur_test->hex[2];
      *(char *)HEX3 = cur_test->hex[3];
      *(char *)HEX4 = cur_test->hex[4];
      *(char *)HEX5 = cur_test->hex[5];
    }else{
      *(char *)HEX0 = cur_test->hex[6];
      *(char *)HEX1 = cur_test->hex[7];
      *(char *)HEX2 = HEX_0;
      *(char *)HEX3 = HEX_0;
      *(char *)HEX4 = HEX_0;
      *(char *)HEX5 = HEX_0;
    }
    if(FD_ISSET(0, &fdsc_read)){
      buf[read(0, buf, BUF_SIZE) - 1] = '\0';
      if(isSameStr(buf, "stop", 4)){
        printf("Stopping...\n");
        test_run = 0;
      }else if(isSameStr(buf, "next", 4)){
        if(next_test(&cur_test)){
          printf("Finish!\n");
          test_run = 0;
        }
      }
    }
    usleep(1000000);
  }
  freeVAddr(V_BASE, fdsc);
  return 0;
}
```
