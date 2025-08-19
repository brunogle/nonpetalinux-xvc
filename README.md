# Intro

XilinxVirtualCable esta pensado para ser usado con el PetaLinux de Xilinx que tiene un device llamado `/dev/uio0` para acceder al debug bridge. Se puede hacer funcionar en cualquier linux accediendo a la memoria mapeada directamente usando `/dev/mem`. El codigo original de Xilinx se puede descargar de aca: https://github.com/Xilinx/XilinxVirtualCable/tree/master/jtag/zynq7000/XAPP1251/src

El unico cambio que se le hizo a esta version es:

```c
...
#define MAP_SIZE 0x10000
#define MAP_ADDR 0x43C00000
...
char *d = "mem";
...
volatile jtag_t* ptr = (volatile jtag_t*) mmap(NULL, MAP_SIZE, PROT_READ | PROT_WRITE, MAP_SHARED, fd_uio, MAP_ADDR);
...
```

Donde `0x43C00000` es la direccion default del debug bridge. Despues se puede compilar con `gcc xvcServer.c -o xvcServer`.

Se puede ejecutar con `xvcstart` y parar con `xvcstop`. Si se ejecuta el XVC sin que este el Debug Bridge mapeado a la direccion correcta o sin que el bitstream este cargado, el CPU se cuelga y hay que reiniciarlo.

# Vivado

Para debuggear con ILA y VIO se tiene que incluir un Debug Bridge conectado al AXI del PS. El Debug Bridge tiene que estar configurado en "From AXI to BSCAN". Los ILA y VIO estan conectados implicitamente

# Compilar

Compilar con `gcc xvcServer.c -o xvcServer` directamente sobre la plataforma del target o buscar una forma de crosscompliar.
