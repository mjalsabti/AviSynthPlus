Avisynth Plus change log
------------------------
Source: https://github.com/AviSynth/AviSynthPlus

For a more logical (non-historical) arrangement of changes see readme.txt

20210706 WIP
------------
- Expr: allow arbitrary variable names (instead of A..Z), up to 256. Do not use existing keywords.
  Usual naming rule: starting with '_' or alpha, continued with '_' or alphanumeric characters.
- Expr: add 'round', 'floor', 'ceil', 'trunc' operators (nearest integer, round down, round up, round to zero)
  Acceleration requires at least SSE4.1 capable processor or else the whole expression is running in C mode.
- Fix: Expr: wrong constant folding optimization when ternary operator and Store-Only (like M^) operator is used together.
  For the expression "sxr 0.5 < 128 192 M^ 255 ?" the expected result must be the same as for "sxr 0.5 < 128 255 ?"
  Since 192 M^ is no-op regarding the expression parts.
- Recognize \' and \b and \v in escaped (e"somethg") string literals (see http://avisynth.nl/index.php/The_full_AviSynth_grammar#Literals)
- Expr: allow TAB, CR and LF characters as whitespace in expression strings
- Enable propSet to accept Clips from function type input, add propSetClip for direct parameter
  Enable propGet to return Clip, add propGetClip for direct parameter
  Clip content support for propGetAsArray propSetArray and propGetAll
- Add "PC.2020" to YUV-RGB conversion matrix set
- ColorBarsHD: use BT.709-2 for +I (Pattern 2), not BT.601
  These are from the SMPTE RP 219-1:2014, but those are also on Wikipedia now: https://en.wikipedia.org/wiki/SMPTE_color_bars
  Former values used BT.601 matrix coeff., which is wrong.
  Also fixed Pattern 1 Green.Y to conform to SMPTE RP 219-1:2014 (133, not 134).
  ColorBars: fixed studio RGB values for -I and +Q for rgb pixel types
- Speedup: Overlay mode "multiply": overlay clip is not converted to 4:4:4 internally when 420 or 422 subsampled format 
  (since only Y is used from that clip)
- Speedup: Overlay mode "multiply": SSE4.1 and AVX2 code (was: C only), Proper rounding in internal calculations
- Fix: ConvertAudio integer 32-to-8 bits C code garbage (regression in 3.7)
- ConvertAudio: Add direct Float from/to 8/16 conversions (C,SSE2,AVX2)
- Fix: ConvertAudio: float to 32 bit integer conversion max value glitch (regression in 3.7)
- Fix: Crash in ColorBars very first frame when followed by ResampleAudio
- Fix: frame property access from C interface (for more info see readme.txt)
- Fix: StackVertical and packed RGB formats: get audio and parity from the first and not the last clip
- RGBAdjust: analyse=true 32 bit float support
- experimental! Fix CUDA support on specific builds (apply lost-during-merge differences from Nekopanda branch), add CMake support for the option.
- Fixes for building the core as a static library- RGBAdjust: analyse=true 32 bit float support
- experimental! Fix CUDA support on specific builds (apply lost-during-merge differences from Nekopanda branch), add CMake support for the option.
- Fixes for building the core as a static library

20210111 3.7.0
--------------
- Haiku support
- PowerPC support
- Support for building the core as a static library (mcmtroffaes)
- Fixes for MinGW-w64 compilation
- Shibatch, TimeStretch, and ImageSeq GCC build support
- Shibatch, TimeStretch, and ImageSeq non-Windows support

20201214 3.6.2-WIP
- Fix: AddBorders did not pass frame properties

20201211 3.6.2-test7
--------------------
- Fix: propSet, propDelete and propClearAll not to ruin visibility of variables
  (property read functions are still kept being runtime only)

20201210 3.6.2-test6
-----------------------
- AviSource: fix test5 regression which refused handling old formats like YV24

20201208 3.6.2-test5
-----------------------
- Resizers: throw error on too small dimensions vs. taps
- Add ShowCRC32 debug filter. Parameters are the same as in ShowFrameNumber
- Overlay: allow 4:1:1 input
- Overlay: fix crash when mask is YUV411 and greymask=false
- Overlay: may work quicker, most input/overlay/mask/output clip format conversions moved to filter constructor
- RemoveAlphaPlane: do nothing on YUY2 instead of throwing an error message
- AviSource: support non-printing characters in fourCC code: allow [number] style, e.g. G3[0][16]
- AviSource: add Y410 (YUVA444P10) format support. Allow 'Y410' pixel_type hints.
- AviSource: decode b64a, b48r, v210, P210, P010, P016, P216, v410, Y416, r210, R10k, v308, v408, Y410 fourCCs natively.
- Fix: Average...: check for valid colorspace (e.g. no AverageB for a YUV clip)
- Add: AverageA
- New: Average...: allow YUY2, RGB24/32/48/64 inputs

20201112 3.6.2-test4
--------------------
- Fix: Overlay: Actual frame of a mask clip would be freed up too early in MT environment
- Fix: ConvertBits to ignore dither parameter instead of throwing error, in a 8 to 8 bit case

20201020 3.6.2-test3
--------------------
- Fix: GeneralConvolution missing internal rounding on 8-16 bit formats

20200831 3.6.2-test2
--------------------
- support for Win10 long file path option
- project: Improve inclusion of the ghc filesystem helper library
- project: Add a GitHub action workflow
- posix: fix crash when autoloading imports
- internally refactored ConvertAudio
- ConvertBits(8): fix dither=1 (floyd) for RGB48/RGB64
- Fix: Blur right side garbage: 16 bit+AVX2+non mod32 width
- Fix: check fn signature with implicite "last" first (3.6 regression)
- Fix: function parameters provided as arrays (e.g. GrunT callback of WriteFileIf)

20200624 3.6.2-test1
------------------
- Fix: ConvertBits (YUV): proper rounding when bit depth is reduced and origin is 10-16 bits
  (added rounder before bit-shift)
- New: Histogram("color2") to support 10+ bits.
  Allow bits=x (x=8,9,10,11,12) parameter for this kind of histogram as well.

20200619 3.6.1
--------------
(summary of test versions since 3.6.0)

- Fix: proper handling of autoload directories precedence:
    PluginDir+ in Software/Avisynth in HKEY_CURRENT_USER (highest priority)
    PluginDir+ in Software/Avisynth in HKEY_LOCAL_MACHINE
    PluginDir2_5 in Software/Avisynth in HKEY_CURRENT_USER
    PluginDir2_5 in Software/Avisynth in HKEY_LOCAL_MACHINE (lowest priority)
  Plugin (dll file name) found in a lower priority folder will not load if a similarly named plugin 
  already existed earlier.
- fix: GeneralConvolution: incorrect parse of negative integer coefficient (added +1)
  Regression since r2772.
- Fix: GeneralConvolution: possible crash when chroma=true for 420 and 422 formats
- Fix: ScriptClip + Runtime function object (which are new in 3.6) under heavy multithreading
- New: Histogram("levels") to allow greyscale
- Fix 3.6 regressions
  - when explicit "return last" was needed when followed by legacy function definition.
  - Windows XP is supported again (thread local storage workaround)
  - Stabilize CPP 2.5 plugins
  - allow forced named arrays usage again from plugins (MP_PipeLine)
- Frame property related constants to match existing enum style in avisynth.h.
  Plus they are not colliding now with VapourSynth's definitions.


20200607 3.6.1-test8
--------------------
- fix: GeneralConvolution: incorrect parse of negative integer coefficient (+1)
  regression since r2772
- fix: GeneralConvolution: possible crash when chroma=true for 420 and 422 formats
- Frame property related constant to match existing enum style in avisynth.h

20200605 3.6.1-test7 (internal)
- pass V3 (2.5) IScriptEnvironment for CPP plugins which directly load avisynth.dll and
  request CreateScriptEnvironment with version <= 3 

20200604 3.6.1-test6
- Fix a 3.6 regression when explicit "return last" was needed when followed by legacy function definition.
- Further Cpp 2.5 plugin compatibility fix: when a v2.5 filter is calling GetFrame from its constructor
  (hdragc)

20200603 3.6.1-test5
- Histogram("levels") to allow greyscale
- Fix 3.6 regression: CPP 2.5 interface runtime functions
- Fix 3.6 regression: forced named arrays usage from plugins (MP_PipeLine)

20200531 3.6.1-test4
- Avisynth CPP 2.5 plugin compatibility patch.
- Windows XP patch (thread local storage workaround)
- Fix: proper handling of autoload directories precedence:
    PluginDir+ in Software/Avisynth in HKEY_CURRENT_USER (highest priority)
    PluginDir+ in Software/Avisynth in HKEY_LOCAL_MACHINE
    PluginDir2_5 in Software/Avisynth in HKEY_CURRENT_USER
    PluginDir2_5 in Software/Avisynth in HKEY_LOCAL_MACHINE (lowest priority)
  Plugin (dll file name) found in a lower priority folder will not load if a similarly named plugin already existed earlier.
- Fix: ScriptClip + Runtime function object crash under multithreading
  e.g. script was crashing with high confidence:
    ColorBars(width=640, height=480, pixel_type="yv12")
    ScriptClip(function[](clip c) { propSetInt("s",function[](clip c) { return current_frame }) })
    Prefetch(100)

20200520 3.6.0
--------------
- Added predefined macros for ARM processors. Tested on Raspberry Pi 4B with the aarch64 image of Ubuntu 20.04.
- Added support for disabling the Intel SIMD intrinsics. Gets automatically disabled on non-x86 targets.
- Added submodule to allow macOS 10.13 and 10.14 to build AviSynth+ with the native Clang compiler
- Fixed some warnings on GCC (wangqr)
- Implemented GetNumPhysicalCPUs on Linux and macOS (wangqr)
- Scripts arrays (Info for plugin writers)
  Plugin parameter definition: allow type+'.' and type+'*' syntax for named array parameters

  Example:
    BlankClip has a named "colors" parameter which accepts one or more "float".
    Definition in function signature:
      [colors]f+
    Usage on the caller side: pass [235.0, 128, 128].
    Checking the proper array size is the plugin's task.

  Note:
    Plugins with named array parameter definition syntax will not work for non-array-aware AviSynth versions.

  Note2:
    Type-free unnamed arrays ".+" or ".*" cannot be followed by additional parameters

  Note3:
    A backward compatible way (AVS 2.6 and non-script-array AviSynth+ versions) of using named or unnamed arrays is to specify a single type as "." and
    the plugin would check the argument type for IsArray

- User defined functions:
  Add array parameter types:

  "array" or "val_array": array of any type.
    When unnamed, then this kind of parameter must be the very last one.
    Unnamed free-typed parametes cannot be followed by any other parameter.
    Translates to ".*" in a plugin parameter definition rule.

  "bool_array" "int_array", "float_array", "string_array", "clip_array", "func_array"
    Translates to "b*", "i*", "f*", "s*", "c*", "f*" in a plugin parameter definition rule.

  Example:

    '''
    a = [1.0, 2.0, 4.2]
    b = [3, 4, 5]
    multi = [a,b]

    sum = Summa(multi[0], multi[1], 2)
    SubTitle(Format({sum}))

    Function Summa(array "x", array "y", int "N")
    {
      sum = 0.0
      FOR(i=0,N-1) {
        sum = sum + x[i] * y[i]
      }
      return sum
    }

    or

    Function Summa(float_array x, float_array y, int "N")
    {
      sum = 0.0
      FOR(i=0,N-1) {
        sum = sum + x[i] * y[i]
      }
      return sum
    }
    '''

- IScriptEnvironment new additions
  Ex-IScriptEnvironment2: no-throw version of Invoke name change to -> InvokeTry
  Ex-INeo Invoke versions to -> Invoke2Try, Invoke3, Invoke3Try
  New (was not implemented in any former Interface): Invoke2 (Exception Thrower version of Invoke2Try)

- Test build published on (20200428)

20200428 3.6.0
--------------
- New: Format function

  Syntax:
    string Format(string format, [value1, value2, ...])

  Parameters
    - string format
      unnamed parameter, the format string
    - zero or more values for format replacement
      Single values are converted to string like in AviSynth String() function.
      If a value is a two dimensional array (only with AviSynth+ arrays enabled) then it can be
      used by a named lookup in the format expression.

  Returns
    formatted string

  Description:

    The format string consists of

        ordinary characters (except { and }), which are copied unchanged to the output,
        escape sequences {{ and }}, which are replaced with { and } respectively in the output, and
        replacement fields.

    Each replacement field has the following format:

        introductory { character;

        (optional)
        arg-id, a non-negative number;
        or:
        identifier which is used for lookup named parameters. (["name", value] construction)
        or:
        valid AviSynth variable name

        final } character.

    If arg-id is a number it specifies the index of the argument in args whose value is to be used for formatting;
    Index is zero based.

    If arg-id is string then it serves as a lookup key from the parameters list given as an array ["name",value] pair.
    If not found, then arg-id is searched among Avisynth variables.

    If arg-id is omitted, the arguments are used in order.
    Mixing manual and automatic indexing is not an error.

    Notes

    It is not an error to provide more arguments than the format string requires:

    Format("{} {}!", "Hello", "world", "something"); // OK, produces "Hello world!"

  Examples:
    By Avisynth variable
      max_pixel_value = 255
      SubTitle(Format("max={max_pixel_value}!"))

    By index:
      SubTitle(Format("{0} {1} {0}", "Home", "sweet"))

    In order:
      SubTitle(Format("{} {} {}", "AviSynth", "+", 2020))

    Array name-value pairs
      SubTitle(Format("maximum={max} minimum={min} max again {max}!", ["max",255], ["min",0]))

- frame properties framework, IScriptEnvironment extension
  - Core and concept ported from VapourSynth - thank you
  - Frame properties are really per-frame data which can only be read and written within runtime functions.
    Source filters can feed into frame properties useful data such as information on colorimetry.
  - frame properties are key-value(s) pairs
    - Key is an alphanumeric identifier.
    - Values can be of single value or array of type
      - 64 bit integers (32 bit integer when converted to AVSValue inside Avisynth+, AVSValue limitation)
      - 64 bit doubles (32 bit float when converted to AVSValue inside Avisynth+, AVSValue limitation)
      - strings (or data) with given length. strings are null terminated
      - frame references (PVideoFrame)
      - clip references (PClip)
    - property setting has 3 modes: 0-replace 1-append 2-touch (see AVSPropAppendMode in avisynth.h)
      0 - single value for that key is replaced
      1 - property is appended (make arrays by calling with mode=1 consecutively)
      2 - touch
  - from plugin writer's point of view:
    - new field in internal VideoFrame struct:
      properties, type is AVSMap* which is basically a reference counting struct for holding array of variants
    - frame property passing along the filter chain
      If your filter is not an in-place one (works with NewVideoFrame instead of MakeWritable) then
      you have to pass properties programatically by using a new version of env->NewVideoFrame: NewVideoFrameP.
      This alternative version of NewVideoFrame has a second PVideoFrame* parameter from which properties will be
      copied upon creating an empty VideoFrame.

      Unfortunately if even one filter in the chain is not passing frame properties then it is a dead end on the info.

      Whether you can (should) use it or not, the Avisynth interface version will tell you. (e.g. >= 8) which should be
      queried (best place: filter constructor)
      Unless you drop support for older avs+ or classic Avisynth 2.6, there will be a transient time when you have to support both world.
      Example:
        Check it:
          has_at_least_v8 = true;
          try { env->CheckVersion(8); } catch (const AvisynthError&) { has_at_least_v8 = false; }
        and use it:
          if (has_at_least_v8) dst = env->NewVideoFrameP(vi, &src); else dst = env->NewVideoFrame(vi);

      Another approach VideoFrame is created as before and later 'copyFrameProps' is used.

    - support functions
    In C interface:
      use avs_new_video_frame_p instead of avs_new_video_frame
      use avs_new_video_frame_p_a instead of avs_new_video_frame_a

      AVS_VideoFrame * AVSC_CC avs_new_video_frame_p(AVS_ScriptEnvironment * p, const AVS_VideoInfo * vi, AVS_VideoFrame * propSrc)
      AVS_VideoFrame * AVSC_CC avs_new_video_frame_p_a(AVS_ScriptEnvironment * p, const AVS_VideoInfo * vi, AVS_VideoFrame *propSrc, int align)

    In avisynth.h:
      enums AVSPropTypes, AVSGetPropErrors and AVSPropAppendMode
      AVSMap* is just a pointer which holds the actual list and values of frame properties.
      You don't need to know what's inside. Access its content through IScriptEnvironment or C interface calls

  - IScriptEnvironment interface is extended! For the first time since classic Avisynth 2.6 moved to IF version 6.
    AVISYNTH_INTERFACE_VERION supporting frame properties is: 8.

    [CPP interface]
    - Summary:
      New: frame propery support with NewVideoFrameP and property getter/setter/info helpers
      Old-New: moved from IScriptEnvironment2:
        GetEnvProperty (note: this is for system properties)
        Allocate, Free (buffer pools)
        GetVar versions distinctly named: GetVarTry, GetVarBool, GetVarInt, GetVarDouble, GetVarString, GetVarLong
      Old-New: moved from IScriptEnvironment2 and INeoEnv
        InvokeTry, Invoke2, Invoke2Try, Invoke3, Invoke3Try
        The "xxxTry" versions are returning bool instead of throwing NotFound if specified function does not exist

        IScriptEnvironment V6 contained only:
          AVSValue Invoke(const char* name, const AVSValue args, const char* const* arg_names = 0)
        New content, different parameter list, non-exception thrower versions
          These versions of Invoke will throw NotFound() exception like "Invoke" does
            AVSValue Invoke2(const AVSValue& implicit_last, const char* name, const AVSValue args, const char* const* arg_names = 0)
            AVSValue Invoke3(const AVSValue& implicit_last, const PFunction& func, const AVSValue args, const char* const* arg_names = 0)
          These versions of Invoke will return false instead of throwing NotFound().
            bool InvokeTry(AVSValue* result, const char* name, const AVSValue& args, const char* const* arg_names = 0)
            bool Invoke2Try(AVSValue* result, const AVSValue& implicit_last, const char* name, const AVSValue args, const char* const* arg_names = 0)
            bool Invoke3Try(AVSValue* result, const AVSValue& implicit_last, const PFunction& func, const AVSValue args, const char* const* arg_names = 0)

    - NewVideoFrame with frame property source:
        PVideoFrame NewVideoFrameP(const VideoInfo& vi, PVideoFrame* propSrc, int align = FRAME_ALIGN);

      Instead of using
        PVideoFrame src = child->GetFrame(n, env);
        PVideoFrame frame = env->NewVideoFrame(vi)

      create new video frame with passing properties by giving a source video frame
        PVideoFrame src = child->GetFrame(n, env);
        PVideoFrame dst = env->NewVideoFrameP(vi, &src);

      All core functions in Avisynth support passing frame properties as a first step.
      (Second and further steps for Avisynth core: use "standardized" frame properties for color matrix and primaries info,
      for field based flag, etc...)

      Note: MakeWritable preserves frame properties

    - copy frame properties from one frame to another: copyFrameProps
    - get property pointer for readonly access: getFramePropsRO
    - get property pointer for read/write access: getFramePropsRW
    - property count helper functions
    - clear of individual properties by key or clear everything
    - property getter and setter functions (by key, key+index, by index). Indexes are starting with 0.
    - examples in conditional.cpp, conditional_functions.cpp

      void copyFrameProps(const PVideoFrame& src, PVideoFrame& dst);
        copies frame properties from source to destination frame

      const AVSMap* getFramePropsRO(const PVideoFrame& frame);
      AVSMap* getFramePropsRW(PVideoFrame& frame);
        get pointer to the property struct for readonly or read/write purposes.
        A single frame can have one set of frame properties, but the set can be shared by multiple frames.

      int propNumKeys(const AVSMap* map);
        returns the number of frame properties for a given AVSMap. Each entry have a unique alphanumeric key.
        Key is the identifier name of the frame property.

      const char* propGetKey(const AVSMap* map, int index);
        Retrieves the key (property identifier / property name) by index. 0 <= index < propNumKeys

      int propDeleteKey(AVSMap* map, const char* key);
        Delete an entry. Returns 0 if O.K.

      int propNumElements(const AVSMap* map, const char* key);
        Queries the internal counter for a given property. Properties can be arrays,
        returned value means the array size. Non array properties return 1.
        Returns -1 if property does not exists.

      char propGetType(const AVSMap* map, const char* key);
        returns the data type.
        Type Enums are avaliable here:
          avisynth.h CPP interface:
            '''
            typedef enum AVSPropTypes {
              ptUnset = 'u',
              ptInt = 'i',
              ptFloat = 'f',
              ptData = 's',
              ptClip = 'c',
              ptFrame = 'v',
            } AVSPropTypes;
            '''
          avisynth_c.h C interface:
            '''
            enum {
              AVS_PROPTYPE_UNSET = 'u',
              AVS_PROPTYPE_INT = 'i',
              AVS_PROPTYPE_FLOAT = 'f',
              AVS_PROPTYPE_DATA = 's',
              AVS_PROPTYPE_CLIP = 'c',
              AVS_PROPTYPE_FRAME = 'v'
            };
            '''

      int64_t propGetInt(const AVSMap* map, const char* key, int index, int* error);
      double propGetFloat(const AVSMap* map, const char* key, int index, int* error);
      const char* propGetData(const AVSMap* map, const char* key, int index, int* error);
      PClip propGetClip(const AVSMap* map, const char* key, int index, int* error);
      const PVideoFrame propGetFrame(const AVSMap* map, const char* key, int index, int* error);

        Property read functions.
        Since properties can be arrays, index should be specified.
        Use index = 0 for getting the value for a non-array property, in reality these
        properties are a size=1 arrays.
        Funtion 'propGetData' can be used for both strings and for generic byte arrays.
        Variable 'error' returns an error code of failure (0=success):
          peUnset: key does not exists
          peType: e.g read integer from a float property,
          peIndex: property array index out of range)
        Error enums are defined here:
          avisynth.h CPP interface:
            typedef enum AVSGetPropErrors {
              peUnset = 1,
              peType = 2,
              peIndex = 4
            } AVSGetPropErrors;
          avisynth_c.h C interface:
            enum {
              AVS_GETPROPERROR_UNSET = 1,
              AVS_GETPROPERROR_TYPE = 2,
              AVS_GETPROPERROR_INDEX = 4
            };

      int propGetDataSize(const AVSMap* map, const char* key, int index, int* error);
        returns the string length or data size of a ptData typed element

      int propSetInt(AVSMap* map, const char* key, int64_t i, int append);
      int propSetFloat(AVSMap* map, const char* key, double d, int append);
      int propSetData(AVSMap* map, const char* key, const char* d, int length, int append);
      int propSetClip(AVSMap* map, const char* key, PClip& clip, int append);
      int propSetFrame(AVSMap* map, const char* key, const PVideoFrame& frame, int append);
        Property setter functions.
        propSetData is used for strings or for real generic byte data.
        propSetData has an extra length parameter, specify -1 for automatic string length.
        Data bytes will be copied into the property (no need to keep data for the given pointer)
        Three propery set modes supported.
        - paReplace: adds if not exists, replaces value is it already exists, losing its old value.
        - paAppend: appends to the existing property (note: they are basically arrays)
        - paTouch: do nothing if the key exists. Otherwise, the key is added to the map, with no values associated.
        See mode enums in:
          avisynth.h CPP interface:
            '''
            typedef enum AVSPropAppendMode {
              paReplace = 0,
              paAppend = 1,
              paTouch = 2
            } AVSPropAppendMode;
            '''
          or
          avisynth_c.h C interface:
            '''
            enum {
              AVS_PROPAPPENDMODE_REPLACE = 0,
              AVS_PROPAPPENDMODE_APPEND = 1,
              AVS_PROPAPPENDMODE_TOUCH = 2
            };
            '''

      const int64_t *propGetIntArray(const AVSMap* map, const char* key, int* error);
      const double *propGetFloatArray(const AVSMap* map, const char* key, int* error);
        Get the pointer to the first element of array of the given key.
        Use 'propNumElements' for establishing array size and valid index ranges.

      int propSetIntArray(AVSMap* map, const char* key, const int64_t* i, int size);
      int propSetFloatArray(AVSMap* map, const char* key, const double* d, int size);
        If an array is already available in the application, these functions provide
        a quicker way for setting the properties than to insert them one by one.
        No append mode can be specified, if property already exists it will be overwritten.
        Return value: 0 on success, 1 otherwise

      void clearMap(AVSMap* map);
        Removes all content from the property set.

      AVSMap* createMap();
      void freeMap(AVSMap* map);
        these are for internal use only

    [C interface]
    - Functions and enums are defined avisynth_c.h
    - New: frame propery support with NewVideoFrameP and property getter/setter/info helpers
    - Old-New: moved from IScriptEnvironment2:
        avs_get_env_property (note: this is for system properties)
        avs_pool_allocate, avs_pool_free (buffer pools)
        avs_get_var versions distinctly named: avs_get_var_try, avs_get_var_bool,
        avs_get_var_int, avs_get_var_double, avs_get_var_string, avs_get_var_long

    - new video frame creator functions:
        avs_new_video_frame_p
        avs_new_video_frame_p_a

      They return and empty video frame but with frame properties copied from the source parameter.

        AVS_VideoFrame * AVSC_CC avs_new_video_frame_p(const AVS_VideoInfo* vi, AVS_VideoFrame *propSrc)
        AVS_VideoFrame * AVSC_CC avs_new_video_frame_p_a(AVS_ScriptEnvironment * p, const AVS_VideoInfo * vi, AVS_VideoFrame *propSrc, int align)

      NULL as propSrc will behave like old avs_new_video_frame and avs_new_video_frame_a

    - AVS_VideoFrame struct extended with a placeholder field for 'properties' pointer
    - avs_copyFrameProps
    - avs_getFramePropsRO, avs_getFramePropsRW
    - avs_propNumKeys, avs_propGetKey, avs_propNumElements, avs_propGetType, avs_propGetDataSize
    - avs_propGetInt, avs_propGetFloat, avs_propGetData, avs_propGetClip, avs_propGetFrame, avs_propGetIntArray, avs_propGetFloatArray
    - avs_propSetInt, avs_propSetFloat, avs_propSetData, avs_propSetClip, avs_propSetFrame, avs_propSetIntArray, avs_propSetFloatArray
    - avs_propDeleteKey, avs_clearMap

    See their descriptions above, names are similar to the CPP ones.

    [AviSynth scripting language]

    Frame properties read-write possible only within runtime functions.
    Input value of setter functions are to be come from the return value of "function objects"

    Property setter function names begin with propSet
      - property value is given by the return value of a function object
        Parameters:
          clip c,
          string key_name,
          function object,
          int "mode"
            0=replace (default), 1=append, 2=touch
            There is no append mode for inserting a full array into the property.

          "propSet" csn[mode]i
            generic property setter, automatic type recognition
          "propSetInt" csn[mode]i
            accepts only integer results from function
          "propSetFloat" csn[mode]i
            accepts only float results from function
          "propSetString" csn[mode]i
            accepts only string results from function
          "propSetArray" csn
            accepts only array type results from function

      - Sets a property, its value is given directly

        "propSet" csi[mode]i
        "propSet" csf[mode]i
        "propSet" css[mode]i
        "propSet" csa
          note: array must contain only the similarly typed values, e.g. cannot mix strings with integers.

        Parameters:
          clip c,
          string key_name,
          value (type of integer, float, string and array respectively),
          int "mode"
            0=replace (default), 1=append, 2=touch
            There is no append mode for inserting a full array into the property.

    Delete a specific property entry
      "propDelete" cs

       parameters:
         clip c,
         string key_name,

    Clear all properties for a given video frame
      "propClearAll" c

       parameters:
         clip c

    Reading properties
       Common parameters:
         clip c,
         string key_name,
         integer "index", (default 0): for zero based indexing array access
         integer "offset" (default 0), similar to the other runtime functions: frame offset (e.g. -1: previous, 2: next next)

      "propGetAny" cs[index]i[offset]i
        returns the automatically detected type
      "propGetInt" cs[index]i[offset]i
        returns only if value is integer, throws an error otherwise
      "propGetFloat" cs[index]i[offset]i
        returns only if value is float, throws an error otherwise
      "propGetString"cs[index]i[offset]i
        returns only if value is string, throws an error otherwise
      "propGetAsArray" cs[offset]i
        returns an array. For a single property array size will be 1. (only in array-aware AviSynth+ versions)


    Reading all properties
      "propGetAll" c[offset]i

      (Only in array-aware AviSynth+ versions)
      Returns all frame properties in an array of [key-value] pairs. Array size will be 'numProps'
      Each key-value pair is contained in a two dimensional subarray.
      If the property value for a given key is an array again then "value" will be an array as well.
      Once you have the array with all properties you can access them with the "associative" feature of AviSynth array access

      ScriptClip("""last.propSet("cica","hello"+String(current_frame)).\
        propSetInt("test_i1",function[](clip c) { return current_frame*3 }).\
        propSet("test_i2", current_frame * 2) """)
      ScriptClip("""p = propGetAll() \
        SubTitle("size:" + String(p.ArraySize()) + " " + \
                           String(p["test_i1"]) + " " + \
                           String(p["cica"]) + " " + \
                           String(p["test_i2"]))""")
      ScriptClip("""p = propGetAll() \
        SubTitle("size:" + String(p.ArraySize()) + " " + \
        String(p[0,1]) + " " + \
        String(p[1,1]) + " " + \
        String(p[2,1]), x=0, y=20)""")

    Lists all properties to screen (a debug filter)
      "propShow" c[size]i[showtype]b

      integer "size" default(16)
        font size to use (the "Text" filter is used for display, sizes are of limited set)

      bool "showtype" default (false)
        if true, the data type in parenthesis appears next to the property key name

      Listing appears as a name = value list. Arrays values are put between [ and ]
      Top line contains number is properties. If no properties found, nothing is displayed.

    Other helper function
      "propGetDataSize" cs[index]i[offset]i
        returns the size of the string or underlying data array

      "propNumElements" cs[offset]i
        returns the array size of a given property. 1=single value

      "propNumKeys" c[offset]i
        returns number of entries (keys) for a frame

      "propGetKeyByIndex" c[index]i[offset]i
        returns the key name for the Nth property (zero based, 0<=index<propNumKeys)

      "propGetType" cs[offset]i
        returns the type of the given key
            unset: 0
            integer: 1
            float: 2
            string: 3
            clip: 4
            frame: 5

  Example 1:
  '''
    ColorBars()

    # just practicing with function objects
    ScriptClip(function[](clip c) { c.Subtitle(String(current_frame)) })

    # write frame properties with function object
    ScriptClip("""propSetInt("frameprop_from_str",func(YPlaneMax))""")
    # write frame properties with traditional script string
    ScriptClip(function[](clip c) { propSetInt("frameluma_sc_func",func(AverageLuma)) })

    # read frame properties (function object, string)
    ScriptClip(function[](clip c) { SubTitle(string(propGetInt("frameprop_from_str")), y=20) })
    ScriptClip("""SubTitle(string(propGetInt("frameluma_sc_func")), y=40)""")

    return last
  '''

  Example 2: (long - require NEW_AVSVALUE build - default on POSIX - becasue of arrays)
      '''
      ColorBars(width=640, height=480, pixel_type="yv12", staticframes=true)

      ScriptClip(function[](clip c) { propSetString("s",function[](clip c) { return "Hello " + string(current_frame) }) })
      ScriptClip(function[](clip c) { propSetString("s",function[](clip c) { return "Hello array element #2 " }, mode=1) })
      ScriptClip(function[](clip c) { propSetString("s",function[](clip c) { return "Hello array element #3 "}, mode=1 ) })

      ScriptClip(function[](clip c) { propSetString("s2",function[](clip c) { return "Another property "} ) })

      ScriptClip(function[](clip c) { propSetInt("s_int",function[](clip c) { return current_frame*1 }) })
      ScriptClip(function[](clip c) { propSetInt("s_int",function[](clip c) { return current_frame*2 }, mode=1) })
      ScriptClip(function[](clip c) { propSetInt("s_int",function[](clip c) { return current_frame*4 }, mode=1 ) })

      ScriptClip(function[](clip c) { propSetFloat("s_float",function[](clip c) { return current_frame*1*3.14 }) })
      ScriptClip(function[](clip c) { propSetFloat("s_float",function[](clip c) { return current_frame*2*3.14 }, mode=1) })
      ScriptClip(function[](clip c) { propSetFloat("s_float",function[](clip c) { return current_frame*3*3.14 }, mode=1 ) })

      ScriptClip(function[](clip c) { propSetArray("s_float_arr",function[](clip c) { return [1.1, 2.2] } ) })
      ScriptClip(function[](clip c) { propSetArray("s_int_arr",function[](clip c) { return [-1,-2,-5] } ) })
      ScriptClip(function[](clip c) { propSetArray("s_string",function[](clip c) { return ["ArrayElementS_1", "ArrayElementS_2"] } ) })
      #ScriptClip("""propDelete("s")""")
      ScriptClip(function[](clip c) {
        y = 0
        SubTitle("Prop Key count =" + String(propNumKeys), y=y)
        y = y + 15
        numKeys = propNumKeys() - 1
        for ( i = 0 , numKeys) {
          propName = propGetKeyByIndex(index = i)
          propType = propGetType(propName)
          SubTitle("#"+String(i) + " property: '" + propName + "', Type = " + String(propType) , y=y)
          y = y + 15

          for(j=0, propNumElements(propName) - 1) {
            SubTitle("element #" + String(j) + ", size = " + String(propType == 3 ? propGetDataSize(propName, index=j) : 0) + ", Value = " + String(propGetAny(propName, index=j)), y = y)
            #SubTitle("element #" + String(j) + " size = " + String(propType == 3 ? propGetDataSize(propName, index=j) : 0) + ", Value = " + String(propGetAny(propName, index=j)), y = y)
            y = y + 15
          }

        }
        return last
      })

      ScriptClip(function[](clip c) {
        a = propGetAsArray("s")
        y = 100
        x = 400
        SubTitle(string(a.ArraySize()), x=x, y=y)
        for(i=0, a.ArraySize()-1) {
          SubTitle("["+String(i)+"]="+ String(a[i]),x=x,y=y)
          y = y + 15
        }
      return last
      })

      # get int array one pass
      ScriptClip(function[](clip c) {
        a = propGetAsArray("s_int")
        y = 440
        x = 400
        SubTitle("Array size=" + string(a.ArraySize()), x=x, y=y)
        y = y + 15
        for(i=0, a.ArraySize()-1) {
          SubTitle("["+String(i)+"]="+ String(a[i]),x=x,y=y)
          y = y + 15
        }
      return last
      })

      # get float array one pass
      ScriptClip(function[](clip c) {
        a = propGetAsArray("s_float")
        y = 440
        x = 200
        SubTitle("Array size=" + string(a.ArraySize()), x=x, y=y)
        y = y + 15
        for(i=0, a.ArraySize()-1) {
          SubTitle("["+String(i)+"]="+ String(a[i]),x=x,y=y)
          y = y + 15
        }
      return last
      })

      # get string array
      ScriptClip(function[](clip c) {
        a = propGetAsArray("s_stringa")
        y = 440
        x = 000
        SubTitle("Array size=" + string(a.ArraySize()), x=x, y=y)
        y = y + 15
        for(i=0, a.ArraySize()-1) {
          SubTitle("["+String(i)+"]="+ String(a[i]),x=x,y=y)
          y = y + 15
        }
      return last
      })
    '''

- New function:
  SetMaxCPU(string feature)

  string "feature"

    "" or "none" for zero SIMD support, no processor flags are reported
    "mmx", "sse", "sse2", "sse3", "ssse3", "sse4" or "sse4.1", "sse4.2", "avx, "avx2"

    parameter is case insensitive.
    Note: "avx2" triggers FMA3 flag as well.

  Processor options w/o any modifier will limit the CPU flag report to at most the processor level.
  When "feature" is ended by '+', relevant processor feature flag will be switched on
  When "feature" is ended by '-', relevant processor feature flag will be removed
  Multiple options can be put in a comma separated list. They will evaluated in that order.

  Examples:
    SetMaxCPU("SSE2") reports at most SSE2 processor (even if AVX2 is available)
    SetMaxCPU("avx,sse4.1-") limits to avx2 but explicitely removes reporting sse4.1 support
    SetMaxCPU("none,avx2+") limits to plain C, then switches on AVX2-only support

- Script array for NEW_AVSVALUE define are working again. (default in Linux build - experimental)
  Memo:
    Compiler define NEW_AVSVALUE means script arrays
    - AVSValue deep copy for arrays (arrays in arrays in ...)
    - untyped and unconstrained element number
    - access with indexes or in a dictionary-like associative way
      array_variable = [[1, 2, 3], [4, 5, 8], "hello"]
      dictionary = [["one", 1], ["two", 2]]
      empty = []
      subarray = array_variable[0]
      val = subarray[2]
      val2 = array_variable[1, 3]
      str = array_variable[2]
      n = ArraySize(array_variable) #3
      n2 = ArraySize(empty) #0
      val3 = dictionary["two"]
    - arrays as filter parameters(named and unnamed)
      new 'a' type or use '.+' or '.*' and check AVSValue IsArray()
    - Concept is incompatible with avs 2.5 plugins due to their 'baked' interface code
    - Demo script (to be cleaned up)
      '''
      ColorBars()
      clip=last
      a = [[1,2],[3,4]]
      aa = [1]
      b = a[1,1] + ArrayGet(a, 1,0) + aa[0]

      empty_array = []
      empty_array_2 = empty_array
      #n3 = empty_array_2.ArrayGet(0) # array index out out range error!

      black_yuv_16 = [0,32768,32768]
      grey_yuv_16 = [32768,32768,32768]
      white_yuv_16 = [65535,32768,32768]
      aSelectColors = [\
        ["black", black_yuv_16],\
        ["grey", grey_yuv_16],\
        ["white",white_yuv_16],\
        ["empty",empty_array]\
      ]
      test_array = [99, 1.0, "this is a string"] # mixed types
      test_array2 = [199, 2.0, "This is a string"]

      n = ArraySize(test_array) # 3
      n2 = ArraySize(empty_array_2) # 0
      sum = FirstNSum(grey_yuv_16,2)
      b = b

      clip = clip.Text(e"Array size = " + String(n) +\
       e"\n Empty array size = " + String(n2) +\
       e"\n sum = " + String(sum) +\
       e"\n b = " + String(b) +\
       e"\n white_yuv_16[1]=" + String(aSelectColors["white"][1]) + \
       e"\n [0]=" + String(ArrayGet(test_array,0)) + \
       e"\n [1]=" + String(ArrayGet(test_array,1)) + \
       e"\n [2]=" + ArrayGet(test_array,2), lsp=0, bold=true, font="info_h")

      return clip

      function FirstNSum(array x, int n)
      {
        a = 0
        for (i=0, x.ArraySize()-1) {
          a = a + x[i]
        }
        return a
      }
      '''
- Fix: Mix/Max Runtime function 32bit float chroma: return -0.5..0.5 range (was: 0..1 range)
- AviSynth+ enhancements by Nekopanda (Neo fork)
  - Allow multiple prefetchers (MT) (mentioned earlier)
  - Multithreading and deadlock fixes for ScriptClip
    (originally I intended to pull only Neo changes which were fixing an old AVS+ bug,
     namely ScriptClip and multithreading. But I was not able to do that without pulling
     nearly everything from Neo)
  - Caching enhancements.
    SetCacheMode(0) or SetCacheMode(CACHE_FAST_START) start up time and size balanced mode
    SetCacheMode(1) or SetCacheMode(CACHE_OPTIMAL_SIZE) slow start up but optimal speed and cache size
    Latter can do wonders especially at really low memory environment
  - ScriptClip and variable stability in multithreading.
    Compared to Neo, I extended it a bit.
    Regarding visibility of variables: I had to change back from Neo's new default behaviour,
    which was similar to GRunT's local=true and yielded incompatibility with the past.
    So scripts written on the assumption of local = false did not work.
    Neo's (valid) point: they would not allow the following script to show the behavior to print "3".
    '''
      # prints 3 - Avisynth default but seems incorrect
      global foo=2
      function PrintFoo(clip c)
      { c.ScriptClip("Subtitle(string(foo))", local = false) }
      Version()
      PrintFoo()
      foo = 3
      last

      # prints 2 - Neo's default behaviour
      global foo=2
      function PrintFoo(clip c)
      { c.ScriptClip("Subtitle(string(foo))", local = true) }
      Version()
      PrintFoo()
      foo = 3
      last
    '''

    So all runtime filters* got a bool "local" parameter which acts same as in GRunT
    * ConditionalSelect, ConditionalFilter, ScriptClip, ConditionalReader, FrameEvaluate,
      WriteFile, WriteFileIf, WriteFileStart, WriteFileEnd

    If local=true the filter will evaluate its run-time script in a new variable scope,
    avoiding unintended sharing of variables between run-time scripts.
    (As of current state (20200328): Neo's WriteFileStart and WriteFileEnd works like
    local=false, in Avisynth compatible mode; all other runtime filters work in local=true mode in Neo)

    In our present Avisynth+ all runtime filters are compatible with the usual behaviour,
    but one can set the other mode by "local=true" parameter. This is when script is passed as string
    Except: when the script parameter is not a string but a function (yes! Nekopanda made it available!)
    then we in classic Avs! (haha, Classic+) kept Neo's new recommendation
    and the default value of "local" is true.

    (as a memo, ConditionalFilter still has the GRunT-like "shortcut" style parameter for bool results which implies "=" and "true"
     and parameter "condvarsuffix" for ConditionalReader)
  - UseVar, special filter, opens a clean variable environment in which only the
    variables in the parameter list can be seen.

  - "escaped" string constants: with e prefix right before the quotation mask
    n"Hello \n" will store actual LF (10) control character into the string
    \n \r \t \0 \a \f \\ and \" are converted
  - Introduce function objects into scripts
    Functions can appear as standard Avisynth variables and parameters (AVSValue type='n')
    https://github.com/nekopanda/AviSynthPlus/wiki/Language-New-Features
    Even with variable capture [] (like in GRuntT args)

  - Filter graph. Switch it on by putting SetGraphAnalysis(true) at the beginning of the script.
    Dump to text file with DumpFilterGraph. E.g. DumpFilterGraph("graph.txt", mode=2)
    Output is in "dot" format, it can be converted to an image with Graphviz as follows:
    '''
    dot -Tsvg graph.txt -o graph.svg
    '''

    SetGraphAnalysis (bool)

      Enables (True) or disables (False) graph node insertion into the instantiated filter.
      To output a filter graph, a graph node must be inserted in the filter.
      When a graph node is inserted, performance may decrease slightly due to the increase of internal function calls.
      (In most cases, there is no observable performance degradation.)

    DumpFilterGraph (clip, string "outfile", int "mode", int "nframes", bool "repeat")

      Outputs a filter graph.

      clip
          Clip to output filter graph
      string outfile = ""
          Output file path
      int mode = 0 *
      int nframes = -1
          Outputs the filter graph when processing the specified frame. The cache size and memory usage of each filter at that time are output together. This is effective when you want to know the memory usage of each filter. If -1, output when DumpFilterGraph is called (before the frame is processed).
      bool repeat = false
          Valid only when nframes> 0. Outputs a filter graph repeatedly at nframes intervals.

  - Frame properties (still from Neo!)
    (experimental, we have planned it in Avs+, probably we'll try to follow the
    VapourSynth methods(?))

20200322 3.5.? (dev)
--------------------
- Fix: Multithreading enhancements and fixes (Nekopanda, from Neo fork)
  - Fix old ScriptClip (runtime filters) issue
    In this example "current_frame" variable was not seen by YDifferenceFromPrevious scripted within SubTitle
    resulting in "ERROR: Plane Difference: This filter can only be used within run-time filters" message
    Now this script finally works:
      SetLogParams("log.txt", LOG_DEBUG)
      ColorBars(width=640, height=480, pixel_type="yv12")
      ScriptClip(last, "Subtitle(String(YDifferenceFromPrevious))")
      Prefetch(4)
  - Fix deadlock of ScriptClip on MT
  - MT improvement
    - Allow multiple Prefetchers
    - Add argument to Prefetch to change # of prefetch frames without changing # of threads

    Prefetch (clip c, int threads, int "frames")

    In the original Plus, you could use only one Prefetch, but you can use any number of CUDA versions.
    Also, an argument has been added to specify the number of frames to prefetch.
    Prefetch (1,4) # Make 1 thread stand and prefetch 4 frames
    By doing so, flexible parallelization configuration is possible, such as pipeline parallelization.

    threads
    Number of threads. If it is 0, it passes without doing anything.

    frames
    Number of frames to prefetch.
    Again, if it is 0, it passes without doing anything.

- Fix: BuildPixelType: chroma subsampling of sample clip was ignored.
- POSIX: better behaviour under non-Windows because of having multiple sized fixed fonts, not only a single size=20 one.
  e.g. MessageClip(), Info(), Version(), ColorYUV "show", internal ApplyMessage
- Text filter:
  - font types with
    - "Terminus" fixed fonts added (12-14-16-18-20-22-24-28-32, regular + bold)
    - "Info_h" good old 10x20 fixed font kept under this name
  - much more international unicode characters (1354), use utf8=true under Windows
  - use fontname parameter (default "Terminus", other choice is "info_h")
  - use font_filename parameter (accepts BDF fonts at the moment - import is probably not too smart but worked for Terminus)
  - use size parameter (12 to 32, if no size is available, a smaller one is chosen but at least the smallest one)
  - new parameter: bold (default false)
- Info() filter: when parameter "size" < 0, font is automatically enlarged over 640x480
  (POSIX limit: minimum size is 12, maximum size is 32 - limited by available fixed fonts)")
- SIL OPEN FONT LICENSE added because of usage of Terminus fonts)
- able to build w/o GDI and font rendering engine under Windows, so that text-overlay filters
  work like in POSIX version of AviSynth+ (mainly for my development test)
  Use with NO_WIN_GDI define.
- Fix: ReplaceStr when the pattern string to be replaced is empty
- New:
  Exist() to have bool utf8 parameter
  This is another function to have utf8 option:
  Usage: b = Exist("Здравствуй.mkv",utf8=true). Avs file is saved as utf8 w/o BOM
- Fix: ColorYUV: really disable variable search when parameter "conditional" is false
- Development:
  ScriptEnvironment::VSprintf: parameter (void *) is changed back to va_list.
  May affect C interface (avs_vsprintf) and CPP interface (ScriptEnvironment::VSprintf)
- Enhanced: Planar RGB to YUV 444 10-14 bits: more precision (32 bit float internally)
- Enhanced: Planar RGB to YUV 444 10-16 bits: AVX2 (speed improvement)

20200302 3.5.0
--------------
- New: Native Linux, macOS, and BSD support.
- Fix: ConvertBits 32->8 for extremely out of range float pixel values.
  When pixel value in a 32 bit float format video was way out of range and greater than 128 (e.g. instead of 0 to 1.0 for Y plane) then the ConvertBits(8) had artifacts.
- Fix potential crash on exit or cache shrink (linux/gcc only?)
- Layer: support RGB24 and RGB48 (internally processed as Planar RGB - lossless pre and post conversion)
- Fix: RGBP to 444 8-14bit right side artifacts at specific widths
- Fix: "scalef" and "scaleb" for 32 bit input, when scale_inputs="floatf" produced wrong result
- Fix: missing rounder in V channel calculation of PlanarRGB->YUV 8-14bits SSE2 code
- Overlay: show error when Overlay is fed with clips with different bit depths
- New function:
  bool IsVersionOrGreater(int majorVersion, int minorVersion [,int bugfixVersion]) function
  Returns true if the current version is equal or greater than the required one in the parameters.
  The function is checking the current version against the given parameters (similar to a Windows API function)
  e.g. IsVersionOrGreater(3, 4) or IsVersionOrGreater(3, 5, 8)
- New: "Expr" helpers:
  - Constants "yrange_min", "yrange_half", "yrange_max"
    Unlike the luma/chroma plane adaptive "range_min", "range_half", "range_max" these constants always report the luma (Y) values
  - new parameter: bool clamp_float_UV (default false)
    this parameter affects clamping of chroma planes: chroma is clamped between 0..1.0 instead of -0.5..0.5s
  - "clamp_float" is not ignored (and set to true) when parameter "scale_inputs" auto-scales 32 bit float type pixels
  - New "yscalef" and "yscaleb" keywords similar to "scalef" and "scaleb" but scaling is forced to use rules for Y (non-UV) planes
  - new allowed value "floatUV" for scale_inputs.
    In short: chroma pre-shift by 0.5 for 32 bit float pixels
    Affects the chroma plane expressions of 32 bit float formats.
    Shifts the input up by 0.5 before processing it in the expression, thus values from -0.5..0.5 (zero centered) range are converted to the 0..1 (0.5 centered) one.
    Since the expression result internally has 0..1.0 range, this then is shifted back to the original -0.5..0.5 range. (since 2.2.20)
    Note: predefined constants such as cmin, cmax, range_min, range_max and range_half will be shifted as well, e.g. the expression will see range_half = 0.5
  - These modifications are similar to Masktools2 2.2.20+
- Fix: TemporalSoften possible access violation after SeparateFields (in general: after filters that only change frame pitch)
- Fix: Shibatch.DLL Access Violation crash when exit when target rate is the same as vi.audio_samples_per_second or audio_samples_per_second is 0
- Build system: Cmake: use platform toolset "ClangCL" for using built-in Clang support of Visual Studio.
  Since VS2019 v16.4 there is LLVM 9.0 support.
  Use: Tools|Get Tools and Features|Add Individual Components|Compilers, build tools, and runtimes
       [X] C++ Clang compiler for Windows
       [X] C++ Clang-cl for v142 build tools (x64/x86)
  (for LLVM Clang installed separately, please use llvm or LLVM - no change in its usage)
- New: AddBorders, LetterBox: new color_yuv parameter like in BlankClip
- Fix: Resizers to really resize alpha channel (YUVA, RGBPA)
- Fix: crash when outputting VfW (e.g. VirtualDub) for YUV444P16, other fixes for r210 and R10k formats
- WavSource: really use "utf8" parameter, fix some debug asserts
- TimeStrech: pass internal errors as Avisynth exception text (e.g. proper "Excessive sample rate!" instead of "unhandled C++ error")

20191021 3.4.0
--------------

- Merges in the MT branch, the current state of pinterf/MT, and packaging fixes
- Development HEAD is the master repo again in https://github.com/AviSynth/AviSynthPlus
- Bumps version to 3.4

20190829 r2915
--------------
- Changed: Trim, FreezeFrame, DeleteFrame, DuplicateFrame, Reverse and Loop are using frame cache again (similar to classic Avs 2.6)
- Enhanced: Expr: faster exp, log, pow for AVX2 (sekrit-twc)
- ConditionalReader: allow empty value in text file when TYPE string
- Fix: Expr: fix non-mod-8 issues for forced RGB output and YUV inputs
- New: AviSource support v308 and v408 format (packed 8 bit 444 and 4444)
- Fix: AviSource v410 source garbage (YUV444P10)
- Fix: Expr: when using parameter "scale_inputs" and the source bit depth conversion occured, predefined constants
  (ymin/max, cmin/max, range_min/max/half) would not follow the new bit depth
- Fix: ConvertToRGB from 32bit float YUV w/ full scale matrixes (pc.601, pc.709, average)
- Fix: FlipHorizontal RGB48/64 artifacts
- Enhanced: a bit quicker FlipHorizontal
- Fix: RGB64 Blur leftmost column artifact
- Enhanced: quicker RGB24/48 to Planar RGB for AVX2 capable processors
- Fix: Strip alpha channel when origin is YUVA and using ConvertToYV12/ConvertToYV16/ConvertToYV24 funtions
- Fix: garbage with ConvertToYUY2 from 8 bit YUVA colorspaces
- Enhanced: Colorbars to accept RGB24, RGB48, YV411 and 4:2:2 formats for pixel_type (now all colorspaces are supported)
- Fix: shifted chroma in ColorBars and ColorBarsHD for YUV444PS
- Fix: ConvertToY8, ConvertToYV12, ConvertToYV16, ConvertToYV24 are now allowed only for 8 bit inputs.
  Formerly these functions were allowed for 10+ bit colorspaces but were not converted to real 8 bit Y8/YV12/16/24.
  Use ConvertToY, ConvertToYUV420, ConvertToYUV422, ConvertToYUV444 instead which are bit depth independent
- New parameter in ColorYUV, RGBAdjust, Overlay, ConditionalReader.
    string "condvarsuffix"
  Allows multiple filter instances to use differently named conditional parameters.
  Prevents collision and overwrite of variables which are used by different ConditionalReader instances.
  See Conditional Variables section of http://avisynth.nl/index.php/ColorYUV.
  How does it work: when reading the global variables, the "condvarsuffix" parameter is appended to the variable name.
  E.g. ColorYUV will read "coloryuv_gain_y_a" instead of "coloryuv_gain_y" when condvarsuffix = "_a" is provided for ColorYUV.
  In the matching ConditionalReader one have to use the modified name as well:
      ConditionalReader("coloryuvoffset.txt", "coloryuv_gain_y", false, CondVarSuffix = "_a") # "_a" is added here by parameter
  or specify the suffixed name directly:
      ConditionalReader("coloryuvoffset.txt", "coloryuv_gain_y_a", false) # "_a" is added here manually

  Example:
    Colorbars(512,256).ConvertToYV12.Trim(0,299)
    ConditionalReader("coloryuvoffset.txt", "coloryuv_gain_y", false)
    a=ColorYUV(cont_y=10, conditional=true)

    Colorbars(512,256).ConvertToYV12.Trim(0,299)
    ConditionalReader("coloryuvoffset2.txt", "coloryuv_gamma_y", false, condvarsuffix="_a")
    b=ColorYUV(cont_y=10, conditional=true, condvarsuffix="_a") # will read coloryuv_gain_y_a, coloryuv_gamma_y_a, etc.

    Stackvertical(a,b)

- Fix: ColorBars: pixel_type planar RGB will set alpha to 0 instead of 255, consistent with RGB32 Alpha channel
- Fix: text colors for YUV422PS - regression since r2728 (zero-centered chroma)
- New: VirtualDub2 to display 8 bit planar RGB (needs up-to-date VirtualDub2 as well)
  VfW interface to negotiate 8 bit Planar RGB(A) with FourCCs: G3[0][8] and G4[0][8], similar to the 10-16 bit logic
  (When you would still stick with RGB24/RGB32/RGB64 export, use OPT_Enable_PlanarToPackedRGB = true)
- Fix: planar RGBA Alpha on VfW was uninitialized because it wasn't filled.
- Layer: big update
  Previously Layer was working only for RGB32 and YUY2. Overlay was used primarily for YUV. Now Layer accepts practically all formats (no RGB24).
  Note that some modes can be similar to Overlay, but the two filters are still different.
  Overlay accepts mask clip, Layer would use existing A plane.
  Overlay "blend" is Layer "add", Overlay "add" is different.
  Lighten and darken is a bit different in Overlay.
  Layer has "placement" parameter for proper mask positioning over chroma.

  - Support for all 8-32 bit Y and planar YUV/YUVA and planar RGB/RGBA formats
    When overlay clip is YUVA and RGBA, then alpha channels of overlay clip are used (similarly to RGB32 and RGB64 formats)
    Non-alpha plane YUV/planar RGB color spaces act as having a fully transparent alpha channel (like the former YUY2 only working mode)

    Note: now if destination is YUVA/RGBA, the overlay clip also has to be Alpha-aware type.
    Now A channel is not updated for YUVA targets, but RGBA targets do get the Alpha updated (like the old RGB32 mode did)
    Todo: allow non-Alpha destination and Alpha-Overlay
  - New parameter: float "opacity" (0.0 .. 1.0) optionally replaces the previous "level". Similar to "opacity" in "Overlay"
    For usage of "level" see http://avisynth.nl/index.php/Layer
    "opacity" parameter is bit depth independent, one does not have to fiddle with it like had to with level (which was maxed with level=257 when RGB32 but level=256 for YUY2/YUV)
  - threshold parameter (used for lighten/darken) is autoscaled.
    Keep it between 0 and 255, same as it was used for 8 bit videos.
  - new parameter: string "placement" default "mpeg2".
    Possible values: "mpeg2" (default), "mpeg1".
    Used in "mul", "darken" and "lighten", "add" and "subtract" modes with planar YUV 4:2:0 or 4:2:2 color spaces (not available for YUY2)
    in order to properly apply luma/overlay mask on U and V chroma channels.
  - Fix some out-of-frame memory access in YUY2 C code
  - Fix: Add proper rounding for add/subtract/lighten/darken calculations. (YUY2, RGB32, 8 bit YUV and 8 bit Planar RGB)
  - Fix: "lighten" and "darken" gave different results between yuy2 and rgb32 when Threshold<>0
    Fixed "darken" for RGB32 when Threshold<>0
    Fixed "lighten" and "darken" for YUY2 when Threshold<>0
    All the above was done by specification:
    Add: "Where overlay is brigher by threshold" => e.g. Where overlay is brigther by 10 => Where overlay > src + 10
    Calculation: alpha_mask = ovr > (src + thresh) ? level : 0;
    Add: "Where overlay is darker by threshold" => e.g. Where overlay is darker by 10 => Where overlay < src - 10
    Calculation: alpha_mask = ovr < (src - thresh) ? level : 0;
    The only correct case of the above was "lighten" for RGB32, even in Classic Avisynth. Note: Threshold=0 was O.K.
  - (Just an info: existing lighten/darken code for YUY2 is still not correct, messing up chroma a bit,
     since it uses weights from even luma positions (0,2,4,...) for U, and odd luma positions (1,3,5,...) for V)
- Avisynth C interface header (avisynth_c.h):
  - cosmetics: functions regrouped to mix less AVSC_API and AVSC_INLINE, put together Avisynth+ specific stuff
  - cosmetics: remove unused form of avs_get_rowsize and avs_get_height (kept earlier for reference)
  - use #ifndef AVSC_NO_DECLSPEC for AVSC_INLINE functions which are calling API functions
  - define alias AVS_FRAME_ALIGN as FRAME_ALIGN (keep the AVS_xxxx naming convention)
  - dynamic loader (avs_load_library) uses fallback mechanism for non-existant Avisynth+ specific functions,
    in order to have Avisynth+ specific API functions safely callable even when connected to classic Avisynth DLL
    The smart fallback mechanism for Avisynth+ specific functions ensures that if the functions are not loadable,
    they will work in a classic Avisynth compatible mode:
    Example#1: e.g. avs_is_444 will call the existing avs_is_yv24 instead
    Example#2: avs_bits_per_component will return 8 for all colorspaces (Classic Avisynth supports only 8 bits/pixel)
    Affected functions and mapping:
      avs_is_rgb48, avs_is_rgb64, avs_is_yuv444p16, avs_is_yuv422p16, avs_is_yuv420p16, avs_is_y16: returns false (0)
      avs_is_yuv444ps, avs_is_yuv422ps, avs_is_yuv420ps, avs_is_y32: returns false (0)
      avs_is_yuva, avs_is_planar_rgb, avs_is_planar_rgba: returns false (0)
      avs_is_444: calls avs_is_yv24 instead
      avs_is_422: calls avs_is_yv16 instead
      avs_is_420: calls avs_is_yv12 instead
      avs_is_y: calls avs_is_y8 instead
      avs_num_components: returns 1 for y8, 4 for RGB32, 3 otherwise
      avs_component_size: returns 1 (1 bytes)
      avs_bits_per_component: returns 8 (8 bits)
- filter "Version": update year, removed avs-plus.net link
- Updated: TimeStretch plugin with SoundTouch 2.1.3 (as of 07.Jan 2019)
- Source/Build system
  - rst documentation update (qyot27) in distrib\docs\english\source\avisynthdoc\contributing\compiling_avsplus.rst
  - GCC-MinGW build (huge thanks to qyot27)
    - various source fixes for MinGW builds,
      Avisynth core compiles fine, except external plugins Shibatch and DirectShowSource
    - CMakeLists.txt updates (user selecteble CPU arch, MinGW things)
    - PluginManager.cpp: [MSVC|GCC] should only load [MSVC|GCC] plugins (qyot27)
      This commit simply tells GCC builds of AviSynth to use the value
      of the PluginDir+GCC registry entry to find plugins, and ignore
      PluginDir and PluginDir+.  Vice-versa for MSVC builds.
      C plugins are an exception to this, since those can be loaded with
      either MSVC- or GCC-built AviSynth+.
    - GCC 8.3 support
      (see in distrib\docs\english\source\avisynthdoc\contributing\compiling_avsplus.rst)
      - Howto: MSYS2/GCC: for windows based build environment see step-by-step instructions:
        https://github.com/orlp/dev-on-windows/wiki/Installing-GCC--&-MSYS2
      - CMake: choose generator "MinGW Makefiles" from CMakeGUI or
          del CMakeCache.txt
          "c:\Program Files\CMake\bin\cmake.exe" -G "MinGW Makefiles" .
        then build with
          mingw32-make -f makefile
      - todo: fix plugin DLL names
  - CMake: Visual Studio 2019 generator support, please update to CMake to 3.14.1
  - Clang (LLVM) support with Visual Studio 2019 (16.0) / 2017 (15.9.9) (Latest Clang is 8.0 at the moment)
    - CMakeLists.txt update
    - Additional source fixes.
    - Source lines for different processor targets (SSSE3 and SSE4.1) are now separated and are compiled using function attributes.
    Clang howto:
    - Install LLVM 8.0 or later (http://releases.llvm.org/download.html, Windows pre-built libraries)
    - Install Clang Power Tools & LLVM Compiler Toolchain (thx fuchanghao)
      - https://marketplace.visualstudio.com/items?itemName=caphyon.ClangPowerTools
      - https://marketplace.visualstudio.com/items?itemName=LLVMExtensions.llvm-toolchain
    - (When using CMakeGUI) After Configure/Specify generator for this project, type LLVM for "Optional Toolset to use (-T option)"
    - Known issues:
      - compiling Avisynth only .lib is generated (used for C api), .exp is missing
      - When generating assembler output linking or other errors can happen
      - LLVM 8.0 has sub-optimal avg_epu8 intrinsic "optimization", which is fixed in LLVM 9.0 (e.g. in snapshot build 21 June 2019)

20181220 r2772
--------------
- Fix: Expr: possible Expr x64 crash under specific memory circumstances
- Fix: Expr: safer code for internal variables "Store and pop from stack" (see: Internal variables at http://avisynth.nl/index.php/Expr)

20181218 r2768
--------------
- New: Expr: allow input clips to have more planes than an implicitely specified output format
  Expr(aYV12Clip, "x 255.0 /", format="Y32") # target is Y only which needs only Y plane from YV12 -> no error
- New: Expr: Y-plane-only clip(s) can be used as source planes when a non-subsampled (rgb or 444) output format implicitely specified
  Expr(Y, "x", "x 2.0 /", "x 3.0 /", format="RGBPS") # r, g and b expression uses Y plane
  Expr(Grey_r, Grey_g, Grey_b, "x", "y 2.0 /", "z 3.0 /", format="RGBPS") # r, g and b expression uses Y plane
- Fix: ConvertToYUY2() error message for non-8 bit sources.
- Fix: Y32 source to 32 bit 420,422,444 (introduced in big the zero-chroma-center transition)
- Fix: ShowY, ShowU, ShowV crash for YUV (non-YUVA) sources
- Speedup: ConvertToY12/16... for RGB or YUY2 sources where 4:4:4 or YV16 intermediate clip was used internally
  (~1.5-2x speed, was a regression in Avs+, use intermediate cache again)
- Fix: Allow ExtractY on greyscale clips
- ImageReader/ImageSource: use cache before FreezeFrame when result is a multiframe clip (fast again, regression since an early AVS+ version)
- Resizers: don't use crop at special edge cases to avoid inconsistent results across different parameters/color spaces
- Fix: Histogram 'classic': rare incomplete histogram shown in multithreading environment
- Fix: ImageReader and ImageWriter: if path is "" then it works from/to the current directory.
- GeneralConvolution: Allow 7x7 and 9x9 matrices (was: 3x3 and 5x5)
- GeneralConvolution: All 8-32 bit formats (was: RGB32 only): YUY2 is converted to/from YV16, RGB24/32/48/64 are treated as planar RGB internally
  Since 32 bit float input is now possible, matrix elements and bias parameter now is of float type.
  For 8-16 bit clips the matrix is converted to integer before use.
- GeneralConvolution: Allow chroma subsampled formats to have their luma _or_ chroma processed. E.g. set chroma=false for a YV12 input.
- GeneralConvolution: new parameters: boolean luma (true), boolean chroma(true), boolean alpha(true)
    Default: process all planes. For RGB: luma and chroma parameters are ignored.
    Unprocessed planes are copied. Using alpha=false makes RGB32 processing faster, usually A channel is not needed.
- GeneralConvolution: MT friendly parameter parsing
- New: UTF8 filename support in AviSource, AVIFileSource, WAVSource, OpenDMLSource and SegmentedAVISource
  All functions above have a new bool utf8 parameter. Default value is false.
- Experimental: new syntax element (by addewyd): assignment operator ":=" which returns the assigned value itself.
  (Assignment within an expression)
  Examples:
    w := h := 256

    b := blankclip(width=w * 2, height = h * 3, length=40, pixel_type="yv12")
    bm = blankclip(width=w, height = w).letterbox(2,0,2,0, color=$ff)
    b

    for(j = 0, 1, 1) {
        for(i = 0, 1, 1) {
            e = 0 + i * 16 + j * 16 * 4
            ce = string(e)
            c = bm.subtitle("Y = 0x" + hex(e) + " " + ce)
            eval("c" + string(i) + string(j) + " := c")
            b := b.overlay(c, x = i * w, y = j * h)
        }
    }

    cx = c00.trim(0, 9) + c01.trim(0, 9) + c10.trim(0, 9) + c11.trim(0, 9)

    b := overlay(cx, x = 0, y = w * 2)

     /* defined NEW_AVSVALUE at build */
    array = [99, 101, "303", cnt := 4]

    for(j = 0, cnt - 1, 1) {
        b := subtitle(string(array[ind := j]), x = 100, y=(j+1) * 20)
    }

    g := b.tstfunc(kf := "first", ks := "second")

    g := subtitle((s := 4) > 0 ? t := "left" : t := "right", y = 100)
    g := subtitle(string(s) + " " + t + " " + ks, y = 150)

    eval("""h := g.subtitle("G", x=200, y = 20)""")

    h.subtitle("H " + string(ind), x = 300, y = 20)


    function tstfunc(c, d, e) {
        if (f := 1 < 2) {
            c.subtitle(string(f) + e, y = 50)
        } else {
            c.subtitle(d, y = 50)
        }
    }



20180702 r2728
--------------
- New: Expr: implement 'clip' three operand operator like in masktools2
  Description: clips (clamps) value: x minvalue maxvalue clip -> max(min(x, maxvalue), minvalue)
- New: Expr: Parameter "clamp_float"
    True: clamps 32 bit float to valid ranges, which is 0..1 for Luma or for RGB color space and -0.5..0.5 for YUV chroma UV channels
    Default false, ignored (treated as true) when scale_inputs scales float
- New: Expr: parameter "scale_inputs" (default "none")

    Autoscale any input bit depths to 8-16 bit for internal expression use, the conversion method is either full range or limited YUV range.

    Feature is similar to the one in masktools2 v2.2.15

    The primary reason of this feature is the "easy" usage of formerly written expressions optimized for 8 bits.

    Use
    - "int" : scales limited range videos, only integer formats (8-16bits) to 8 (or bit depth specified by 'i8'..'i16')
    - "intf": scales full range videos, only integer formats (8-16bits) to 8 (or bit depth specified by 'i8'..'i16')
    - "float" or "floatf" : only scales 32 bit float format to 8 bit range (or bit depth specified by 'i8'..'i16')
    - "all": scales videos to 8 (or bit depth specified by 'i8'..'i16') - conversion uses limited_range logic (mul/div by two's power)
    - "allf": scales videos to 8 (or bit depth specified by 'i8'..'i16') - conversion uses full scale logic (stretch)
    - "none": no magic

    Usually limited range is for normal YUV videos, full scale is for RGB or known-to-be-fullscale YUV

    By default the internal conversion target is 8 bits, so old expressions written for 8 bit videos will probably work.
    This internal working bit-depth can be overwritten by the i8, i10, i12, i14, i16 specifiers.

    When using autoscale mode, scaleb and scalef keywords are meaningless, because there is nothing to scale.

    How it works:
    - This option scales all 8-32 bit inputs to a common bit depth value, which bit depth is 8 by default and can be
      set to 10, 12, 14 and 16 bits by the 'i10'..'i16' keywords
      For example: scale_inputs="all" converts any inputs to 8 bit range. No truncation occurs however (no precision loss),
      because even a 16 bit data is converted to 8 bit in floating point precision, using division by 256.0 (2^16/2^8).
      So the conversion is _not_ a simple shift-right-8 in the integer domain, which would lose precision.
    - Calculates expression (lut, lut_xy, lut_xyz, lut_xyza)
    - Scales the result back to the original video bit depth.
      Clamping (clipping to valid range) and converting to integer occurs here.

    The predefined constants such as 'range_max', etc. will behave according to the internal working bit depth

    Warning#1
    This feature was created for easy porting earlier 8-bit-video-only lut expressions.
    You have to understand how it works internally.

    Let's see a 16bit input in "all" and "allf" mode (target is the default 8 bits)

    Limited range 16->8 bits conversion has a factor of 1/256.0 (Instead of shift right 8 in integer domain, float-division is used or else it would lose presision)

    Full range 16->8 bits conversion has a factor of 255.0/65535

    Using bit shifts (really it's division and multiplication by 2^8=256.0):
      result = calculate_lut_value(input / 256.0) * 256.0
    Full scale 16-8-16 bit mode ('intf', 'allf')
      result = calculate_lut_value(input / 65535.0 * 255.0 ) / 255.0 * 65535.0

    Use scale_inputs = "all" ("int", "float") for YUV videos with 'limited' range e.g. in 8 bits: Y=16..235, UV=16..240).
    Use scale_inputs = "allf" (intf, floatf) for RGB or YUV videos with 'full' range e.g. in 8 bits: channels 0..255.

    When input is 32bit float, the 0..1.0 (luma) and -0.5..0.5 (chroma) channel is scaled
    to 0..255 (8 bits), 0..1023 (i10 mode), 0..4095 (i12 mode), 0..16383(i14 mode), 0..65535(i16 mode) then back.

    Warning#2
    One cannot specify different conversion methods for converting before and after the expression.
    Neither can you specify different methods for different input clips (e.g. x is full, y is limited is not supported).

- Fix: Expr: expression string order for planar RGB is properly r-g-b like in original VapourSynth version, instead of counter-intuitive g-b-r.
- Fix: Expr: check subsampling when a different output pixel format is given
- Fix: ColorYUV: round to avoid green cast on consecutive TV<>PC
- Enhanced: Limiter to work with 32 bit float clips
- Enhanced: Limiter new parameter bool 'autoscale' default false.
  If set, minimum/maximum luma/chroma values are treated as they were in 8 bit range (but non-integer values are allowed), limiter will autoscale it.
  Default: does not scale at all, parameters are used as-is. Parameters now are of float type to handle 32 bit float values.
- New: function bool VarExist(String variable_name)
  Checks if a variable exists
  Returns true if variable exists even if it holds 'Undefined', false otherwise
- Fix: RGBAdjust memory leak when used in ScriptClip
- Enhanced: RGBAdjust new parameter: conditional (like in ColorYUV)
  The global variables "rgbadjust_xxx" with xxx = r, g, b, a, rb, gb, bb, ab, rg, gg, bg, ag are read each frame, and applied.
  It is possible to modify these variables using FrameEvaluate or ConditionalReader.
- Enhanced: RGBAdjust: support 32 bit float ('analyze' not supported, 'dither' silently ignored)
- Enhanced: AviSource to support more formats with 10+ bit depth.
  http://avisynth.nl/index.php/AviSource

  When pixel_type is not specified or set to "FULL", AviSource will try to request the formats one-by-one in the order shown in the table below.

  When a classic 'pixel_type' shares more internal formats (such as YUV422P10 first tries to request the v210 then P210 format)
  you can specify one of the specific format directly. Note that high bit-depth RGBP is prioritized against packed RGB48/64.

  The 'FourCCs for ICDecompressQuery' column means that when a codec supports the format, it will serve the frame in that one, Avisource then will convert it to the proper colorspace.

  Full support list (* = already supported):
  'pixel_type' Avs+ Format   FourCC(s) for ICDecompressQuery
  YV24         YV24          *YV24
  YV16         YV16          *YV16
  YV12         YV12          *YV12
  YV411        YV411         *Y41B
  YUY2         YUY2          *YUY2
  RGBP10       RGBP10        G3[0][10]  r210  R10k
  r210         RGBP10        r210
  R10k         RGBP10        R10k
  RGBP         RGBP10        G3[0][10]  r210  R10k
               RGBP12        G3[0][12]
               RGBP14        G3[0][14]
               RGBP16        G3[0][16]
               RGBAP10       G4[0][10]
               RGBAP12       G4[0][12]
               RGBAP14       G4[0][14]
               RGBAP16       G4[0][16]
  RGB32        RGB32         *BI_RGB internal constant (0) with bitcount=32
  RGB24        RGB24         *BI_RGB internal constant (0) with bitcount=24
  RGB48        RGB48         BGR[48]    b48r
  RGB64        RGB64         *BRA[64]   b64a
  Y8           Y8            Y800       Y8[32][32]   GREY
  Y            Y8            Y800       Y8[32][32]   GREY
               Y10           Y1[0][10]
               Y12           Y1[0][12]
               Y14           Y1[0][14]
               Y16           Y1[0][16]
  YUV422P10    YUV422P10     v210       P210
  v210         YUV422P10     v210
  P210         YUV422P10     P210
  YUV422P16    YUV422P16     P216
  P216         YUV422P16     P216
  YUV420P10    YUV420P10     P010
  P010         YUV422P10     P010
  YUV420P16    YUV420P16     P016
  P016         YUV422P16     P016
  YUV444P10    YUV444P10     v410
  v410         YUV444P10     v410
- Changed (finally): 32bit float YUV colorspaces: zero centered chroma channels.
  U and V channels are now -0.5..+0.5 (if converted to full scale before) instead of 0..1
  Note: filters that relied on having the U and V channel center as 0.5 will fail.
  Why: the old UV 0..1 range was a very-very early decision in the high-bitdepth transition project. Also it is now
  compatible with z_XXXXX resizers (zimg image library, external plugin at the moment).
- New function: bool IsFloatUvZeroBased()
  For plugin or script writers who want to be compatible with pre r2672 Avisynth+ float YUV format:
    Check function availablity with FunctionExists("IsFloatUvZeroBased").
    When the function does not exists, the center value of 32 bit float U and V channel is 0.5
    When IsFloatUvZeroBased function exists, it will return true (always for official releases) if U and V is 0 based (+/-0.5)
- Fix: RGB64 Turnleft/Turnright (which are also used in RGB64 Resizers)
- Fix: Rare crash in FrameRegistry
- Enhanced: Allow ConvertToRGB24-32-48-64 functions for any source bit depths
- Enhanced: ConvertBits: allow fulls-fulld combinations when either clip is 32bits
  E.g. after a 8->32 bit fulls=false fulld=true:
  Y: 16..235 -> 0..1
  U/V: 16..240 -> -0.5..+0.5
  Note: now ConvertBits does not assume full range for YUV 32 bit float.
  Default values of fulls and fulld are now true only for RGB colorspaces.
- Fix: couldn't see variables in avsi before plugin autoloads (colors_rgb.avsi issue)
- Fix: LoadVirtualdubPlugin: Fix crash on exit when more than one instances of a filter was used in a script
- New: LoadVirtualdubPlugin update:
  - Update from interface V6 to V20, and Filtermod version 6 (partial)
  - VirtualDub2 support with extended colorspaces
    Allow RGB24, RGB48, RGB64 besides RGB32
    AutoConvert 8 bit Planar RGB to/from RGB24, RGBPA to/from RGB32 (lossless)
    AutoConvert RGB48 and 16 bit Planar RGB(A) to/from RGB64 (lossless)
    Support YUV(A) 8 bits: YV12, YV16, YV24, YV411, YUVA420P8, YUVA422P8, YUVA444P8
    Support YUV(A) 10-16 bits (properly set "ref_x" maximum levels, no autoconvert)
  - Supports prefetchProc2 callback (API >= V14 and prefetchProc2 is defined) for multiple input frames from one input clip
    PrefetchFrameDirect and PrefetchFrame are supported. PrefetchFrameSymbolic not supported
  - Supports prefetchProc callback (API >= V12 and prefetchProc is defined)
  - Supports when filter changes frame count of the output clip
  - Extra filter parameter added at the end of filter's (unnamed) parameter list
    Imported Virtualdub filters are getting and extra named parameter to the end:
      String [rangehint]
    This parameter can tell the filter about a YUV-type clip colorspace info
    Allowed values:
      "rec601": limited range + 601
      "rec709": limited range + 709
      "PC.601": full range + 601
      "PC.709": full range + 709
      ""      : not defined (same as not given)
    Parameter will be ignored when clip is non-YUV
    How it works: the hint will _not_ change the internal VirtualDub colorspace
    constant (e.g. kPixFormat_YUV420_Planar -> kPixFormat_YUV420_Planar_709 will not happen).
    Instead the base color space is kept and colorSpaceMode and colorRangeMode will set in PixmapLayout.formatEx.
    Filter can either use this information or not, depending on supported API version and its implementation.
    E.g. Crossfade(20,30) -> Crossfade(20,30,"rec601") though this specific filter won't use it.
- New function: BuildPixelType

  Creates a video format (pixel_type) string by giving a colorspace family, bit depth, optional chroma subsampling and/or a
  template clip, from which the undefined format elements are inherited.
  "[family]s[bits]i[chroma]i[compat]b[oldnames]b[sample_clip]c"

  string family: YUV, YUVA, RGB, RGBA, Y
  int bits: 8, 10, 12, 14, 16, 32
  string chroma: for YUV(A) 420,422,444,411. Ignored for RGB(A) and Y
  bool compat (default false): returns packed rgb formats for 8/16 bits (RGB default: planar RGB)
  bool oldnames (default false): returns YV12/YV16/YV24 instead of YUV420P8/YUV422P8/YUV444P8
  clip sample_clip: when supported, its format is overridden by specified parameters (e.g. only change bits=10)

  Example#1: define YUV 444 P 10

    family = "YUV"
    bits = 10
    chroma = 444
    compat = false
    oldformat = false
    s = BuildPixelType(family, bits, chroma, compat, oldformat)
    BlankClip(width=320,height=200,length=len,pixel_type=s,color=$008080).Info()

  Example#2: Change only the bit depth of the format to 16

    newbits = 16
    c = last
    s = BuildPixelType(bits=newbits, sample_clip=c)
    BlankClip(width=320,height=200,length=len,pixel_type=s,color=$008080).Info()

- Source: move to c++17, 'if constexpr' requires. Use Visual Studio 2017 (or GCC 7?). CMakeLists.txt changed.
- Source: C api: AVSC_EXPORT to dllexport in capi.h for avisynth_c_plugin_init
- Source: C api: avs_is_same_colorspace VideoInfo parameters to const
- Project struct: changelog to git.
- Include current avisynth header files and def/exp file in installer, when SDK is chosen

20180328 r2664
--------------
-   Fix: YUY2 Sharpen overflow artifacts - e.g. Sharpen(0.6)
-   Fix: Levels: 32 bit float shift in luma
-   Fix: Merge sse2 for 10-14bits (regression)
-   Fix: AVX2 resizer possible access violation in extreme resizes (e.g. 600->20)
-   Fix: 32bit float PlanarRGB<->YUV conversion matrix
-   Fix: VfW: fix b64a output for OPT_Enable_b64a=true
-   Enhanced: VfW output P010 and P016 conversion to SSE2 (VfW output is used by VirtualDub for example)
-   Enhanced: ColorYUV: recalculate 8-16 bit LUT in GetFrame only when changed frame-by-frame (e.g. in autowhite)
-   Enhanced: ConvertBits 32->8 sse2/avx2 and 32->10..16 sse41/avx2 (8-15x speed)

Not included, preliminary for the near future:
-   Big change: 32 bit float YUV formats, U and V are now zero based.
    Internally YUV 32 bit float chroma center became 0.0 (the neutral value which is 128 in the 8-bit world)
    Like in VapourSynth or in avsresizer using z.lib image library.
    'Expr' changes are affecting built-in constants/operators when used in chroma plane of a 32bit clip.
    - 'cmin', 'cmax' return the zero-based shifted versions of the 16 and 240 (8 bit) values
    - For U and V planes, constant 'range_half' results in 0.0 instead of the old 0.5
    - 'scaleb' will also give zero-based result when found in an expression for chroma plane
      (e.g. for a 32 bit float clip the '128 scaleb' will result in 0.0 instead of 128/255 for U and V planes)
      But 'scalef' when the target or source of the constant conversion is 32bits, remains independent from the plane type.
    - 'range_max' is 0.5 for 32 bit float chroma
    - new constant 'range_min', which is -0.5 for 32 bit float chroma, (0 otherwise)

    Additional warning: when you move 32bit float U or V plane to Y using CombinePlane, you have to be sure
    that your filters do not rely on this new Y plane being in 0..1 range. Or else convert it by using Expr("x 0.5 +") to the 0..1 range
    Similarly: ExtractU and ExtractV will simply return the unaltered chroma planes, which are now zero-centered


20180302 r2636
--------------
-   Fix: Blur/Sharpen crashed when YUY2.width<8, RGB32.width<4, RGB64.width<2
-   ColorYUV: don't apply TV range gamma for opt="coring" when explicit "PC->TV" is given
-   ColorbarsHD: 32bit float properly zero (0.5)-centered chroma

20180301 r2632
--------------
  - Fix: IsInterleaved returned false for RGB48 and RGB64 (raffriff42)
  - Fix: SubTitle for Planar RGB/RGBA: wrong text colors (raffriff42)
  - Fix: Packed->Planar RGB conversion failed on SSE2-only computers (SSSE3 instruction used)
  - Enhanced: Blur, Sharpen
    AVX2 for 8-16 bit planar colorspaces (>1.35x speed on i7-7770)
    SSE2 for 32 bit float formats (>1.5x speed on i7-7770)
  - Fix: Resizers for 32 bit float rare random garbage on right pixels (simd code NaN issue)
  - Enhanced: Completely rewritten 16bit and float resizers, much faster (and not only with AVX2)
  - Enhanced: 8 bit resizers: AVX2 support
  - Enhanced: Speed up converting from RGB24/RGB48 to Planar RGB(A) - SSSE3, approx. doubled fps
  - New: ConvertFPS supports 10-32 bits, planar RGB(A), YUV(A)
  - New script function: int BitSetCount(int[, int, int, ...])
    Function accepts one or more integer parameters
    Returns the number of bits set to 1 in the number or the total number of '1' bits in the supplied integers.
  - Cherry-picking from StainlessS' great RT_xxxx collection/and raffriff42 utils
  - Modded script function: Hex(int , int "width"=0)
    - New "width" parameter
    - result is in uppercase
    Width is 0 to 8, the _minimum_ width of the returned string. (8 hex digit is the max of Avisynth32 bit integer)
    When width is 0 or not supplied then string length is a minimum needed.
    Function now returns hex string in uppercase, instead of lowercase.
    Example: Hex(255,4) returns "00FF".
  - Modded script function: HexValue(String, "pos"=1)
    - new pos parameter
    Returns an int conversion of the supplied hexadecimal string.
    Conversion will cease at the first non legal number base digit, without producing an error
    Added optional pos arg default=1, start position in string of the HexString, 1 denotes the string beginning.
    Will return 0 if error in 'pos' ie if pos is less than 1 or greater than string length.
  - Modded script function: ReplaceStr(String, String, String[, Boolean "sig"=false])
    - New parameter: sig for case insensitive search (Default false: exact search)
      The uppercase/lowercase rules come from the current active code page of the OS.
  - New script functions: TrimLeft, TrimRight, TrimAll for removing beginning/trailing whitespaces from a string.
    Whitespaces: Tab (9), space (32), nbsp (160)
  - New in ColorYUV:
    New parameter: bool f2c="false".
    When f2c=true, the function accepts the Tweak-like parameters for gain, gamma and contrast
    E.g. use 0/0.5/1.0/1.5/2.0/3.0 instead of -256/-128/0/128/256/512
  - New/Fixed in ColorYUV:
    Parameter "levels" accepts "TV". (can be "TV->PC", "PC->TV", "PC->TV.Y")
    Now gamma calculation is TV-range aware when either
    - levels is "TV->PC" or
    - coring = true or
    - levels is "TV" (new - no level conversion but gamma will know proper handling)
    Previously gamma was properly calculated only for PC range.
  - New in ColorYUV:
    32 bit float support.
    - 32 bit float uses the Expr filter (8-16 bits is LUT-based). The expression is dynamically assembled for each plane, internal precision is float.
    - One can specify bits=32 when showyuv=true -> test clip in YUV420PS format
    For 32 bit clips "loose min" and "loose_max" (omitting the extreme 1/256 population from dark and bright pixels) statistics are computed
    by splitting the 0..1 into 65536 uniform ranges.
 - Modded: remove "scale" parameter from ConvertBits.
   It was introduced at the very beginning of the 10+bit development, for 32bit float conversion - never used
 - Enhanced: VfW: exporting Y416 (YUV444P16) to SSE2.
 - 8-16 bit YUV chroma to 32 bit float: keep middle chroma level (e.g. 128 in 8 bits) at 0.5.
   Calculate chroma as (x-128)/255.0 + 0.5 and not x/255.0 (Note: 32 bit float chroma center will be 0.0 in the future)
 - New: Histogram parameter "keepsource"=true (raffriff42)
   keepsource = false returns only the Histogram w/o the original picture.
   Affects "classic", "levels" and "color", "color2", ignored (n/a) for the other modes
 - New: Histogram type "color" to accept 8-32bit input and "bits"=8,9,..12 display range
 - New: Histogram parameter "markers"=true
   When markers = false:
   For "classic": no "half" level line and no invalid luma zone coloring
   For "levels":  no "half" dotted line, no coloring (neither for YUV nor for RGB)
   Ignored for the others at the moment.

20171226 r2580
----------------
  - Fix (workaround): Merge: Visual Studio 2017 15.5.1/2 generated invalid AVX2 code
    The x86 path of Merge Average was crashing due to bad code generation in the 8 bit version. Seems that thought
    Visual Studio Team was proudly announcing optimizations in their compiler (15.5.x line), it had victims.
  - Fix: Temporalsoften 10-14 bits: an SSE 4.1 instruction was used for SSE2-only CPU-s (Illegal Instruction on Athlon XP)

20171219 r2574
----------------
  - Fix: MaskHS created inverse mask. Regression after r2173
  - Build: changed avisynth.h, strict C++ conformity with Visual Studio 2017 /permissive- flag
  - Installer (finally)
  - Expr:
    - new: Indexable source clip pixels by relative x,y positions like x[-1,1]
    - new functions: sin cos tan asin acos atan
    - new operator: % (modulo)
    - new: Variables: uppercase letters A..Z for storing and reuse temporary results, frequently used computations.
    - new: predefined expr variables 'frameno', 'time', 'width', 'height'
    - fix: jitasm code generation at specific circumstances

20171115 r2544
  - Expr: fix "scalef" for 10-16 bits
  - Expr optimization: eliminate ^1 +0 -0 *1 /1

20171114 r2542
  - New filter: Expr
    Ported from Vapoursynth, with a lot of additional tweaks
    e.g. AVX2, recognize constant fill, spatial absolute and relative coordinates as input, recognize plane copy,
         no GetFrame for unused clips, converts ^2, ^3, ^4 into faster multiplication,
         converts x^0.5 into sqrt(x), borrow specific syntax elements from masktools2 (scaling, operators - but not all of them) to have partial
         compatibility in widely used existing expression strings (see readme.txt)

    clip Expr(clip c[,clip c2, ...], string expr [, string expr2[, string expr3[, string expr4]]] [, string format]
        [, bool optSSE2][, bool optAVX2][, bool optSingleMode])

  - Add: Levels: 32 bit float format support
  - Fix: RGB (full scale) conversion: 10-16 bits to 8 bits rounding issue; pic got darker in repeated 16<->8 bit conversion chain
  - Fix: ConvertToY: remove unnecessary clamp for Planar RGB 32 bit float
  - Fix: RGB ConvertToY when rec601, rec709 (limited range) matrix. Regression since r2266
  - Optimized: Faster RGB (full scale) 10-16 bits to 8 bits conversion when dithering
  - Other: Default frame alignment is 64 bytes (was: 32 bytes). (independently of AVX512 support)
  - Build:
    Built with Visual Studio 2017, v141_xp toolset
    Note that DLL will work When you have VS2015 Update 3 redistributable.

    Download Visual Studio 2017 Redistributable from here (replaces and compatible with VS2015 redist)

    X64:
    https://go.microsoft.com/fwlink/?LinkId=746572

    x86:
    https://go.microsoft.com/fwlink/?LinkId=746571

  - Experimental x64 builds for test (internal offsets from int to size_t)
  - Source: avisynth_c.h (C interface header file) changed:
    Optional define SIZETMOD. Experimental. Offsets are size_t instead of int (x64 is different!)
    Fix: avs_get_row_size calls into avs_get_row_size_p, instead of direct field access
    Fix: avs_get_height calls into avs_get_row_size_p, instead of direct field access.

20170629 r2508
  - Fix TemporalSoften: threshold < 255 (probably since r1576)

20170608 r2506
  - Fix CombinePlanes: feeding YV16 or YV411 target with Y8 sources

20170603 r2504
  - fix XP support broken in r2502

20170602 r2502
  - fix: (Important!) MT_SERIALIZED mode did not always protect filters (regression since r2069)
    Such filters sometimes were called in a reentrant way (like being MT_NICE_FILTER), which
    possibly resulted in using their internal buffers parallel.
  - Fix: ImageWriter crash when no '.' in provided filename
  - Fix: Overlay: correct masked blend: keep exact clip1 or clip2 pixel values for mask extremes 255 or 0.
    Previously 0 became 1 for zero mask, similarly 255 changed into 254 for full transparency (255) mask
  - New: script functions: StrToUtf8, StrFromUtf8: Converting a 8 bit (Ansi) string to UTF8 and back.
  - New: PluginManager always throws error on finding wrong bitness DLL in the autoload directories
  - Modified: increased x64 default MemoryMax from 1GB to 4GB, but physicalRAM/4 is still limiting
  - Modified: allow conversions between RGB24/32/48/64 (8<->16 bits) w/o ConvertBits
  - Added VS2017 and v141_xp to CMakeList.txt

20170529 r2489
  - fix: memory leak in CAVIStreamSynth (e.g. feeding vdub)
  - fix: ConvertToY for RGB64 and RGB48

20170528 r2487
  - Blur, Sharpen 10-16 bits planar and RGB64: SSE2/SSE4 (2x-4x speed)
  - New script function: int GetProcessInfo([int type = 0])
    Without parameter or type==0 the current bitness of Avisynth DLL is returned (32 or 64)
    With type=1 the function can return a bit more detailed info:
    -1: error, can't establish
    0: 32 bit DLL on 32 bit OS
    1: 32 bit DLL on 64 bit OS (WoW64 process)
    2: 64 bit DLL
  - Fix: Blur width=16 (YV12 width=32)
  - Fix: Overlay Lighten: artifacts when base clip and overlay clip have different widths (regression since r2290)
  - Fix: YUY2 HorizontalReduceBy2 did nothing if target width was not mod4
  - ImageReader: 16 bit support; "pixel_type" parameter new formats "RGB48", "RGB64" and "Y16"
  - ImageWriter: 16 bit support; save RGB48, RGB64, Y16, planar RGB(A) 8 and 16 bit formats
    (note: greyscale through devIL can be corrupt with some formats, use png)
  - ImageWriter: flip greyscale images vertically (except "raw" format)
  - SubTitle: new parameter "font_filename" allows using non-installed fonts
  - (project can be compiled using gcc)
  - Allows opening unicode filenames through VfW interface (virtualdub, MPC-HC)
  - Script function Import: new parameter bool "utf8" to treat the filenames as UTF8 encoded
    (not the script text!)
  - SubTitle: new parameter bool "utf8" for drawing strings encoded in UTF8.
      Title="Cherry blossom "+CHR($E6)+CHR($A1)+CHR($9C)+CHR($E3)+CHR($81)+CHR($AE)+CHR($E8)+CHR($8A)+CHR($B1)
      SubTitle(Title,utf8=true)
  - New script functions: ScriptNameUtf8(), ScriptFileUtf8(), ScriptDirUtf8(),
    they return variables $ScriptNameUtf8$, $ScriptFileUtf8$ and $ScriptDirUtf8$ respectively

  Knows issues:
  - Filters with MT_SERIALIZED sometimes can get called in a reentrant way
  - Runtime Script functions under MT


20170316 r2455
  - Fix: IsY() script function returned IsY8() (VideoInfo::IsY was not affected)
  - New: ConvertBits, dither=1 (Floyd-Steinberg): allow any dither_bits value between 0 and 8 (0=b/w)

20170310 r2440
  - Fix Merge for float formats
  - Fix error text formatting under wine (_vsnprintf_l issue)
  - Fix Regression: YUY2 UToY copied V instead of U, since August, 2016 (v2150)

  - faster Merge: float to sse2 (both weighted and average)
  - faster ordered dither to 8bit: SSE2 (10x speed)

  - ColorBars allows any 4:2:0, 4:4:4 formats, RGB64 and all planar RGB formats
  - ColorBarsHD accepts any 4:4:4 formats
  - Dithering: Floyd-Steinberg
    Use convertBits parameter dither=1: Floyd-Steinberg (was: dither=0 for ordered dither)
  - Dithering: parameter "dither_bits"
    For dithering to lower bit depths than the target clip format
    Usage: ConvertBits(x, dither=n [, dither_bits=y])
    - ordered dither: dither_bits 2, 4, 6, ... but maximum difference between target bitdepth and dither_bits is 8
    - Floyd-Steinberg: dither_bits 1, 2, 4, 6, ... up to target bitdepth - 2
    (Avisynth+ low bitdepth, Windows 3.1 16 bit feeling I was astonished that dither_bits=6 still resulted in a quite usable image)
  - Dithering is allowed from 10-16 -> 10-16 bits (was: only 8 bit targets)
  - Dithering is allowed while keeping original bit-depth. clip10 = clip10.ConvertBits(10, dither=0, dither_bits=8)
    (you still cannot dither from 8 or 32 bit source)
  - ConditionalFilter syntax extension like Gavino's GConditional: no "=" "true" needed
  - Revert: don't give error for interlaced=true for non 4:2:0 sources (compatibility, YATTA)
  - CombinePlanes: silently autoconvert packed RGB/YUY2 inputs to planar
  - ConvertBits: show error message on YV411 conversion attempt: 8 bit only
  - ConvertBits: Don't give error message if dither=-1 (no dithering) is given for currently non-ditherable target formats
  - Script function: IsVideoFloat. returns True if clip format is 32 bit float. For convenience, same as BitsPerComponent()==32
  - ConvertToDoubleWidth and ConvertFromDoubleWidth: RGB24<->RGB48, RGB32<->RGB64
  - New MT mode: MT_SPECIAL_MT. Specify it for MP_Pipeline like filters, even if no Prefetch is used (MP_Pipeline issue, 2 fps instead of 20)


20170202 r2420
  - CombinePlanes:
    When there is only one input clip, zero-cost BitBlt-less subframes are used, which is much faster.

    e.g.: casting YUV to RGB, shuffle RGBA to ABGR, U to Y, etc..
    Target planes that are not specified, preserve their content.

    Examples:
    combineplanes(clipRGBP, planes="RGB",source_planes="BGR") # swap R and B
    combineplanes(clipYUV, planes="GBRA",source_planes="YUVA",pixel_type="RGBAP8") # cast YUVA to planar RGBA
    combineplanes(clipYUV, planes="Y",source_planes="U",pixel_type="Y8") # extract U

  - fix: SubframePlanarA (available in IScriptEnvironment2)
  - faster: Difference-type conditional functions: Simd for 10-16 bits
  - Fix: MinMax-type conditional functions (min, max, median): return float value for float clips
  - ConvertToPlanarRGB(A):
    PlanarRGB <-> PlanarRGBA is now allowed
  - ConvertToPlanarRGB(A):
    YUY2 source is now allowed (through automatic ConvertToRGB proxy)
  - faster: RemoveAlphaPlane (subframe instead of BitBlt copy)
  - Overlay: "Blend" native greyscale mode: process y plane only w/o conversion
  - Overlay: automatic use444=false for "blend"/"luma"/"chroma"
    for inputs: 420/422/444 and any RGB, lossless Planar RGB intermediate for PackedRGB
    Overlay/mask auto-follows input clip format.
    For compatibility: when greymask=true (default) and mask is RGB then mask source is the B channel
  - faster: RGB48->RGB64 SSSE3 (1,6x), RGB64->RGB48 SSSE3 (1.5x speed)
  - faster: RGB24,RGB48->PlanarRGB: uses RGB32/64 intermediate clip
  - Histogram "levels": allow RGB24/32/48/64 input.
    Display R, G and B channels instead of Y, U and V
    Reminder 1: "levels" for Planar RGB was working already
    Reminder 2: Histogram "levels" and "Classic" allows bits=xx parameter, xx=8..12
                If "bits" is specified then Histogram is drawn with 9..12 bits precision. Get a wide monitor though :)
  - ConvertBits: new parameters, partially for the future.
    bool fulls, bool fulld

    For YUV and greyscale clips the bit-depth conversion uses simple bit-shifts by default.
    YUV default is fulls=false

    RGB is converted as full-stretch (e.g. 0..255->0..65535)
    RGB default is fulls=true

    If fulld is not specified, it takes the value of fulls.
    Use case: override greyscale conversion to fullscale instead of bit-shifts

    Note 1: conversion from and to float is always full-scale
    Note 2: alpha plane is always treated as full scale
    Note 3: At the moment you cannot specify fulld to be different from fulls.


20170119 r2397
  - TemporalSoften: Planar RGB support
  - TemporalSoften: much faster average mode (thres=255)
    radius=1 +70%, radius=2 +45%,
    16bit: generally 7-8x speed (SSE2/4 instead of C)
  - SeparateColumns: 10-16bit,float,RGB48/64
  - WeaveColumns: 10-16bit,float,RGB48/64,PlanarRGB(A)
  - AddAlphaPlane: fix function parameter type list, clip type did not work
  - Internals: add SubframePlanarA to IScriptEnvirontment2 for frames with alpha plane
  - SwapUV: YUVA support
  - ConvertToRGB32/64: copy alpha from YUVA
  - SeparateRows,SeparateFields: PlanarRGB(A),YUVA support
  - WeaveRows: PlanarRGB(A), YUVA
  - Weave (fields,frames): YUVA,PlanarRGB(A)
  - Crop: Fast crop possible for frames with alpha plane (subframe)
  - AddBorders missing l/r/top/bottom vs. subsampling check for YUVA
  - Fix: YUVA->PlanarRGBA and YUVA42x->444 missing alpha plane copy
  - YUV444->RGB48/64: fast intermediate PlanarRGB(A) then RGB48/64 (not C path)
  - RGB48/64->YUV4xx target: Planar RGB intermediate (instead of C, 10x faster)

20170111 r2380
  - Overlay: Merge the source of Chroma and Luma modes into Blend mode
             Modes Blend,Luma and Chroma now support all bit depths and 444 conversionless (use444=false) mode
  - Overlay: fix SSE2 Blend for mask+opacity for 10-16 bits
  - VfW: (vdubmod14 VfW test)
    Fix: YUV444P16 or YUVA444P16 to fourcc Y416
         if alpha channel is present, it will we copied, else filled with FFFF
    Fix: VDubPlanarHack is checked only for 8 bit YUV planar sources
20170110 r2372  (vdubmod14 VfW test)
  - New: SSE2/SSE4 for 10-16 bit <-> 10-16 bit Planar RGB (and Alpha plane) full scale conversions
    (needed for automatic planar RGB -> packed RGB VfW conversions)
  - VfW:
    Fixed: Y3[10][10],Y3[10][16] fourcc's byte order
    New: Planar RGB(A) (MagicYUV)
         10,12,14,16 bits: G3[0][10], G4[0][10], G3[0][12], G4[0][12], G3[0][14], G4[0][14], G3[0][16], G4[0][16]
    New: YUV444P16 to fourcc Y416
    New: Automatic conversion of 12, 14 and float YUV formats to 16 bit for 4:2:0 and 4:2:2
         Note: OPT_Enable_Y3_10_16 is still valid as if format was originally 16 bits
    New: Automatic conversion of 10, 12, 14 and float YUV formats to 16 bit for 4:4:4
    New: Conversion of 10, 12, 14 and float planar RGB formats to RGB64
         when global Avisynth variable Enable_PlanarToPackedRGB is true
    New: Conversion of 8 bit planar RGB formats to RGB24
         when global Avisynth variable Enable_PlanarToPackedRGB is true
    New: Conversion of 8 bit planar RGBA formats to RGB32
         when global Avisynth variable Enable_PlanarToPackedRGB is true

    Note: use OPT_VDubPlanarHack=true for YV16 and YV24 for old VirtualDub

    Supported formats:
    BRA[64],b64a,BGR[48],P010,P016,P210,P216,Y3[10][10],Y3[10][16],v210,Y416
    G3[0][10], G4[0][10], G3[0][12], G4[0][12], G3[0][14], G4[0][14], G3[0][16], G4[0][16]

    Default format FourCCs:
    RGB64: BRA[64]
    RGB48: BGR[48]
    YUV420P10: P010
    YUV420P16: P016
    YUV422P10: P210
    YUV422P16: P216
    YUV444P16 and YUVA444P16: Y416
    Planar RGB  10-16 bit: G3[0][10], G3[0][12], G3[0][14], G3[0][16]
    Planar RGBA 10-16 bit: G4[0][10], G4[0][12], G4[0][14], G4[0][16]

    Global variables to override default formats:
    OPT_Enable_V210 = true --> v210 for YUV422P10
    OPT_Enable_Y3_10_10 = true --> Y3[10][10] for YUV422P10
    OPT_Enable_Y3_10_16 = true --> Y3[10][16] for YUV422P16
    OPT_Enable_b64a = true --> b64a for RGB64
    Enable_PlanarToPackedRGB = true --> RGBP8->RGB24, RGBAP8->RGB32, all other bit depths to RGB64

20170109 r2367dev
  - VfW: BRA[64],b64a,BGR[48],P010,P016,P210,P216,Y3[10][10],Y3[10][16],v210
    experimental!
    Default format FourCCs:
    RGB64: BRA[64]
    RGB48: BGR[48]
    YUV420P10: P010
    YUV420P16: P016
    YUV422P10: P210
    YUV422P16: P216
    Global variables to override default formats:
    OPT_Enable_V210 = true --> v210 for YUV422P10
    OPT_Enable_Y3_10_10 = true --> Y3[10][10] for YUV422P10
    OPT_Enable_Y3_10_16 = true --> Y3[10][16] for YUV422P16
    OPT_Enable_b64a = true --> b64a for RGB64
  - Overlay: blend for float format
  - Overlay: blend: SSE4 for 10-16 bit, SSE2 for float
  - AddAlphaPlane: also accepts clip with Y-only or alpha (YUVA/PRGBA/RGB32/64) for alpha source
    (was: optional int/float mask value)

20170104 r2359dev
  - Overlay:
    new parameter: bool use444 (default true for compatibility)

    Parameter is valid only for "blend" at the moment

    When set to false, and base clip/overlay clip is 420/422/Planar RGB, the overlay core
    does not convert to and back from YV24 (4:4:4 in general)

    mask can be either greyscale or can be any planar YUV when greymask=true (default)

    Works for Planar RGB, so blending is basically lossless for this format (no YUV conversion)

    todo: support for other modes, convert packed RGB to planar RGB internally instead of YUV
  - Overlay:
    Add fast 4:2:0<->4:4:4 conversion, e.g. YV16<->YV24 (only fast YUY2 and YV12 existed so far)
    instead of invoking generic core conversion
  - Overlay:
    10-16bit SSE2/SSE4 for 420/422<->444 conversions
  - Info() made a bit more compact.
    Bit depth info moved after color space info
    Does not display pre-SSE2 CPU flags when at least AVX is available
    Display AVX512 flags in separate line (would be too long)
  - new CPU feature constants (see cpuid.h and avisynth_c.h)
    Detect FMA4 and AVX512F,DQ,PF,ER,CD,BW,VL,IFMA,VBMI
  - new script function:
    string ReplaceStr(string s, string pattern, string replacement)
    Function is case sensitive, parameters are unnamed
  - new script function
    int NumComponents(clip)
    returns 1 for grayscale, 3 for YUVxxx, YUY2, planar RGB or RGB24/RGB48, 4 for YUVAxxx, Planar RGBA or RGB32/64
  - new script function:
    bool HasAlpha(clip)
    returns true when clip is YUVA, Planar RGBA, or packed RGB32 or RGB64

20161222 r2347dev
  - CombinePlanes addition: setting target to default RGBP(A)/YUV(A), when inputs are greyscale and no source planes are given
    Decision is made by the target plane characters, if they are like R,G,B then target video format will be planar RGB
    Same logic applies for YUV.
    Example:
    Y1, Y2 and Y3 are greyscale clips
    Old, still valid: combineplanes(Y1, Y2, Y3, planes="RGB", source_planes="YYY", pixel_type="RGBP8")
    New:              combineplanes(Y1, Y2, Y3, planes="RGB") # result: Planar RGB
  - Fix: ScriptClip would show garbage text when internal exception occurs instead of the error message

20161211 r2343dev
  - Overlay: use y offset when greymask=true (fix)
  - Fix DV chroma positioning (UV swapped), interlaced parameter check for 4:2:0
    (fix by IanB in classic Avisynth)
  - BitBlt in 32 bit Avisynth:
    for processors with AVX or better ignore tricky isse memcpy replacement, trust in memcpy (test)
    (x64 is O.K., it always used memcpy)
  - Merge: use stream_load for AVX2
  - VDubFilter.dll:
    convert 'd' double and 'l' long typed parameters to 'f' float and 'i' int for poor AviSynth
    thus allowing the usage of such virtualdub filters
  - remove script array (new AVSValue schema) feature, cannot make it compatible with Invoke from v2.5 plugins
    until I figure out a workaround or such plugins would slowly distinct.

20161208 r2337dev
  - [feature temporarily removed, script arrays are incompatible until 2.5 plugins are supported]
    C interface array compatibility vol#2 (zero size arrays)
    (new AVSValue schema problem)
  - Merge, MergeChroma, MergeLuma: AVX2 (planar)
  - Possibly a bit faster text overlay

20161207 r2333dev
  - Overlay fix

20161206 r2331dev
  - YUY2 PlaneToY finally works
  - C interface compatible array-type AVSValue handling (new AVSValue schema problem)

20161205 r2327dev
  - [feature temporarily removed, script arrays are incompatible until 2.5 plugins are supported]
    BlankClip parameter "colors" accepts exact color values to use
    Color order: Y,U,V,A or R,G,B(,A)
    These color values are used as-is, not scaled or converted in any way.
    Reason: old colors parameter is int (32 bit) cannot hold three or four 16 bit or float values
    Example: BlankClip(width=1920,height=1080,length=1000,pixel_type="RGB64", colors=[64000,32768,1231,65535])
  - ExtractY, PlaneToY("Y") accepts YUY2 clip
  - ExtractR, ExtractG, ExtractB, ExtractA,
    and
    PlaneToY("R"), PlaneToY("G"), PlaneToY("B"), PlaneToY("A")
    functions are accepting packed RGB input (RGB24/32/48/64)
    They are converted to planar RGB on-the-fly before plane extraction
  - Histogram "levels" works from Planar RGB.
    Color legends show R, G and B.
    bits=8..12 parameter is still available for finer ultra-wide histogram display

20161201 r2322dev
  - [feature temporarily removed, script arrays are incompatible until 2.5 plugins are supported]
    constant script arrays
    (note: feature was temporarily removed in r2443)
    array_variable = [[1,2,3],[4,5,8],"hello"]
    dictionary = [["one",1],["two",2]]
    empty = []
    subarray = array_variable[0]
    val = subarray[2]
    val2 = array_variable[1,3]
    str = array_variable[2]
    n = ArraySize(array_variable) #3
    n2 = ArraySize(empty) #0
    val3 = dictionary["two"]
  - arrays as filter parameters (named and unnamed):
    new 'a' type or use '.' (any) and check AVSValue IsArray()
    todo: maybe .+ or *+ syntax?
    (note: feature was temporarily removed in r2443)
  - Planar RGB <-> YUV: SSE2 (SSE4)
  - Planar RGB <-> Packed RGB32/64: SSE2

20161120:
  make PlanarRGB TurnLeft, TurnRight work again.
  (stricter check in PlaneToY)

20161119
  Fix: PlaneToY("Y")

20161116

  Replaced AToY8, GToY8, BToY8, RToY8
  We have ExtractX, (X = R,G,B,Y,U,V,A) instead.
  That Y8 ending was too confusing.
  See new test build

20161110 Avisynth plus additions
New functions

  AToY8 , same as PlaneToY("A") for planar RGBA or YUVA
  RToY8 , same as PlaneToY("R") for planar RGB
  GToY8 , same as PlaneToY("G") for planar RGB
  BToY8 , same as PlaneToY("B") for planar RGB

  They work the same way as UToY8 and VToY8 and ConvertToY did.
  They convert to greyscale keeping the original bit-depth, not only 8 bit!
  Y8 naming was kept, because UToY and VToY already existed.

Extended function
  old: YToUV(clip clipU, clip clipV [, clip clipY ] )
  new: YToUV(clip clipU, clip clipV [, clip clipY [, clip clipA] ] )

  YToUV accepts optional alpha clip after Y clip

  Example
    U = source.UToY8()
    V = source.VToY8()
    Y = source.ConvertToY()
    A = source.AddAlphaPlane(128).AToY8()
    # swaps V, U and A, Y
    YToUV(V,U,A,Y).Histogram("levels").Info().RemoveAlphaPlane()

New function
  CombinePlanes(clip1 [,clip2, clip3, clip4], string planes [, string source_planes, string pixel_type, string sample_clip])

  Combines planes of source clip(s) into a target clip

  If sample_clip is given, target clip properties are copied from that clip
  If no sample_clip is provided, then clip1 provides the template for target clip
  An optional pixel_type string (e.g."YV24", "YUV420PS", "RGBP8") can override the base video format.

  If the source clip count is less than the given planes defined, then the last available clip is
  used as a source.

  string planes
    the target plane order (e.g. "YVU", "YYY", "RGB")
    missing target planes will be undefined in the target

  string source_planes (optional)
    the source plane order, defaulting to "YUVA" or "RGBA" depending on the video format

  Examples#1
    #combine greyscale clips into YUVA clip
    U8 = source.UToY8()
    V8 = source.VToY8()
    Y8 = source.ConvertToY()
    A8 = source.AddAlphaPlane(128).AToY8()
    CombinePlanes(Y8, U8, V8, A8, planes="YUVA", source_planes="YYYY", sample_clip=source) #pixel_type="YUV444P8"

  Examples#2
    # Copy planes between planar RGB(A) and YUV(A) without any conversion
    # yuv 4:4:4 <-> planar rgb
    source = last.ConvertBits(32) # 4:4:4
    cast_to_planarrgb = CombinePlanes(source, planes="RGB", source_planes="YUV", pixel_type="RGBPS")
    # get back a clip identical with "source"
    cast_to_yuv = CombinePlanes(cast_to_planarrgb, planes="YUV", source_planes="RGB", pixel_type="YUV444PS")

  Examples#3
    #create a black and white planar RGB clip using Y channel
    #source is a YUV clip
    grey = CombinePlanes(source, planes="RGB", source_planes="YYY", pixel_type="RGBP8")

  Examples#4
    #copy luma from one clip, U and V from another
    #source is the template
    #sourceY is a Y or YUV clip
    #sourceUV is a YUV clip
    grey = CombinePlanes(sourceY, sourceUV, planes="YUV", source_planes="YUV", sample_clip = source)

