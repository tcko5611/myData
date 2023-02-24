---
date : 2022-06-28 19:47
priority : 4

---
# Metadata
Status :: #Status/Validate 
Type :: #CF/GUI 
Topics :: [P10241015-3443](https://jira.internal.synopsys.com/browse/P10241015-3443) [[wv, debugging]]
# Note
## test case
/slowfs/hs_scr1/tcko/customfault/sae/jira/3443_wv
## wv library
/remote/sweifs/PE/products/cktsim/t2022.06_rel/image/handsoff/T-2022.06_WVADV_PROD/api_image/calc_api/
## primewave code
/slowfs/sandwork3/gwang/clients/cswv_rel/primewave/src/sonoma/waveform_commands/expr.cpp
## binary
/remote/swefs1/PE/products/cktsim/t2022.06_dev/clientstore/cktsim_t2022.06_dev/object_root/bin-linux64/primewave-g
/remote/swefs1/PE/products/cktsim/t2022.06_dev/clientstore/cktsim_t2022.06_dev/object_root/bin-linux64/wv-g1
## flow
expr::execute(expr.cpp) -> waveform::signal_data::evaluate_expression(signal_tcl_object.cpp) -> readerapi::wfCalculate()(readerapi.cpp) -> ::wfCalculate (from wv api)

./sx/test/capi_unit_test/unit_test.c
## enviroment variables
* set env LD_LIBRARY_PATH=/remote/swefs1/PE/products/cktsim/t2022.06_dev/image/nightly/wvadv_optimize/D20220815_7757054/Testing/platforms/linux64/lib
* setenv TCL_LIBRARY /global/apps/cx_2022.06-2/etc/tcl
* setenv SW_SX_TCL_LIB /global/apps/cx_2022.06-2/platforms/linux64/lib/libtcl8.6.so
## EQBUF data
``` cpp
typedef struct _eqbuf {
  char *pSigName; // singal name, could we assign it just for identify? -> eqn1
  char *pEquation; // expression ? -> ymax(sig1) + ymin(sig2)+sig3
  HSIG ResultWaveform; // what is this field? -> you do not need to fill it, it is to store the result wave
  double Scalar_Y; // Do we need to fill it? -> you do not need to fill it, it is to store the result scalar REAL part
  double Scalar_I; // Do we need to fill it? -> you do not need to fill it, it is to store the result scalar IMAG part
  char Scalar_V[1024]; // Do we need to fill it? -> you do not need to fill it, it is to store the result scalar LOGIC part
  INT4 Result_Type; // Do we need to fill it? -> you do not need to fill it, it is to store the result Type,
  INT4 SweepScalar; // Do we need to fill it? -> you do not need to fill it, it is to store the result is sweep scalar or not
  INT4 ErrorPos; // Do we need to fill it? è you do not need to fill it, it is to store the eqn syntax errpos
  INT4 SweepMeasXParmIdx; // Do we need to fill it? -> It is to specify the flatten parameter idx， you need to specify it as “SWEEP_MEAS_NO_FLAT” if you do not want to flatten the sweep measure results.
  HSIG  (*FindSignal)(char*,ADDR,INT4*,double*,double*,char*,INT4*,INT4*); // call back for schematic name to wavename? Can we skip this and change the expression(sch->wave) into pEquation? -> you can not ignore it. but you can just use our find signal CB to create  your own signal call back easily if you have translated the signal into waveform_based name.
  INT4 (*GobjRegCB)(ADDR); // Do we need to fill it? -> You can ignore it
  double (*GetSigValue)(HSIG); // Do we need to fill it? -> This one is to get the value just in case some arguments in the equation should be treated as scalar value.
  ADDR UserData; // if we don’t use FindSignal, could we skip it? -> if you want to use our find signal CB, you need to set the waveform file as the UserData
  char ErrMsg[1024]; // Do we need to fill it? -> you do not need to specify it, it is to store the error message during calculation
  char SweepMeasXParmName[100]; // Do we need to fill it? -> You do not need to specify it, it is to store the x-param name
  INT2 reqScalarSig; // Do we need to fill it? -> you can ignore it
  char MeasResultName[100]; // Do we need to fill it? -> if it is a “.meas”, you need to specify the measure name
  INT4 From_GUI; // Do we need to fill it?  -> You can specify it as 0
  INT4 NoLic_Required; // 1 or other ?  -> It depends on the license check required or not.
  HMEM hemm; // You can ignore it.
  } EQBUF;
```
## Work flow
call back funtion code position:
\<primewave\>/src/sx/test/capi_unit_test/src/sx/test/capi_unit_test/unit_test.c
```cpp
HSIG
apiFindVariable(
char   *pname,
ADDR    userdata,
INT4   *piscalar,
double *preal,
double *pimag,
char   *perr,
INT4   *ptidx,
INT4   *pcidx)
  {
  HSIG     sig;
  char     buf[1024] = {0},*psig,*pend;
  FIND_ARG arg;

  strncpy(buf,pname, sizeof(buf) - strlen(buf) - 1);
  capiStrStripQuote2(buf);

  if(strchr(buf,'.') && !strstr(buf,"S-Par")) {
    *piscalar = WF_CAL_WDF;
    return _File;
    }
  // strip off v( prefix
  if(!strncasecmp("V(",buf,2)) {
    psig = buf+2;
    // strip off trailing )
    pend = psig + strlen(psig) - 1;
    if(*pend == ')')
      *pend = (char)0;
    }
  else
    psig = buf;

// TODO: complex....
  if(!strncasecmp("i(",buf,2)) {
    psig = buf+2;
    // strip off trailing )
    pend = psig + strlen(psig) - 1;
    if(*pend == ')')
      *pend = (char)0;

    memset(&arg, 0, sizeof(FIND_ARG));
    arg.Type = WF_SIGTYPE_CURR;
    sig = wrFindSignal(_File,psig,&arg);
    if(!sig) {
      arg.Type = WF_SIGTYPE_CX_CURR;
      sig = wrFindSignal(_File,psig,&arg);
      }
    }
  else if(!strncasecmp("Pr(rload)",buf,9) || !strncasecmp("S-Par",buf,5)){
    sig = wrFindSignal(_File,psig,NULL);
    }
  else {
    memset(&arg, 0, sizeof(FIND_ARG));
    arg.Type = WF_SIGTYPE_VOLT;
    sig = wrFindSignal(_File,psig,&arg);
    if(!sig) {
      arg.Type = WF_SIGTYPE_CX_VOLT;
      sig = wrFindSignal(_File,psig,&arg);
      }
    if(!sig) {
      arg.Type = WF_SIGTYPE_LOGIC;
      sig = wrFindSignal(_File,psig,&arg);
      }
    if(!sig)
      sig = wrFindSignal(_File,psig,NULL);
    }
  if(sig) {
    *piscalar = WF_CAL_SIGNAL;
    return sig;
    }
  else {
    if(!strcmp(buf,"c_pi") || !strcmp(buf,"c_e")) {
      *piscalar = WF_CAL_UNDEFINED;
      *preal = *pimag = 0.0;
      return NULL;
      }
    // CDE code should
    *piscalar = WF_CAL_SCALARX;
    *preal = *pimag = 4.0;
    return NULL;
    }
  }
```
## Synmake
* new libray location: /remote/swefs1/PE/products/cktsim/t2022.06_dev/clientstore/cktsim_t2022.06_dev_build1/dependent/wvadv/latest/capi
* new include file and library: include/capi.h, linux64/libcapi_o.a, linux64_g.a
* Master.make used variables :
	* WVADV_ROOT : dependent/wvadv/latest (system define)
	* WDF_PATH = $(WVADV_ROOT)
	* WDF_OPT = \$(WDF_PATH)/\$(TARGET_ARCH)/libgrpw-O.a
	* LIBFSDB_NFFR = -lnffr, LIBFSDB_NSYS = -lnsys
	* LIBFSDB_LIB_PATH : system define