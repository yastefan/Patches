/*{
  "DESCRIPTION": "Your shader description",
  "CREDIT": "by you",
  "CATEGORIES": [
    "Your category"
  ],
  "INPUTS": [
    {
      "NAME": "iChannel0",
      "TYPE": "image"
    },
    {
      "NAME": "DEPTH",
      "TYPE": "float",
      "DEFAULT": 50,
      "MAX": 100,
      "MIN": 5
    },
    {
      "NAME": "EXTRUSION",
      "TYPE": "float",
      "DEFAULT": 0.1,
      "MAX": 1,
      "MIN": -1
    },
    {
      "NAME": "SAMPLERADIUS",
      "TYPE": "float",
      "DEFAULT": 12,
      "MAX": 20,
      "MIN": 0
    },
    {
      "NAME": "SAMPLES",
      "TYPE": "float",
      "DEFAULT": 64,
      "MAX": 128,
      "MIN": 16
    },
    {
      "NAME": "CHUNKINESS",
      "TYPE": "float",
      "DEFAULT": -8,
      "MAX": 20,
      "MIN": -20
    },
    {
      "NAME": "WARPFACTORA",
      "TYPE": "float",
      "DEFAULT": 0.1,
      "MAX": 5,
      "MIN": 0.025
    },
    {
      "NAME": "WARPFACTORB",
      "TYPE": "float",
      "DEFAULT": 0,
      "MAX": 5,
      "MIN": 0
    },
    {
      "NAME": "OVERLOAD",
      "TYPE": "float",
      "DEFAULT": 0,
      "MAX": 1,
      "MIN": 0
    },
    {
      "NAME": "TIMEOFFSET",
      "TYPE": "float",
      "DEFAULT": 0.5,
      "MAX": 1,
      "MIN": 0
    },
    {
      "NAME": "TIMEDILATION",
      "TYPE": "float",
      "DEFAULT": 1,
      "MAX": 1,
      "MIN": -1
    },
    {
      "NAME": "GAMMA",
      "TYPE": "float",
      "DEFAULT": 2.2,
      "MAX": 10,
      "MIN": 0
    }
  ]
}*/

// Based off "Interstellar" by TekF: https://www.shadertoy.com/view/Xdl3D2

// SAMPLERADIUS is the distance to sample data fromt he centre of the input
// WARPFACTORA smears the dots into streaks
// WARPFACTORB offsets the RGB channels
// DEPTH scales the output in the Z-AXIS
// EXTRUSION is the amount to displace points based on brightness of input
// OVERLOAD is a multiplier for the DEPTH
// TIMEOFFSET allows you to pick a point in time. Useful for still images when TIMEDILATION is 0.
// TIMEDILATION allows for speed of zoom in apps that donot support SPEED control
// GAMMA adjusts contrast

vec3 iResolution = vec3(RENDERSIZE, 1.0);
float iGlobalTime = TIME;

const float tau = 6.28318530717958647692;

// Gamma correction
// #define GAMMA (2.2)

vec3 ToLinear( in vec3 col )
{
	// simulate a monitor, converting colour values into light values
	return pow( col, vec3(GAMMA) );
}

vec3 ToGamma( in vec3 col )
{
	// convert back into colour values, so the correct light will come out of the monitor
	return pow( col, vec3(1.0/GAMMA) );
}

vec4 Noise( in ivec2 x )
{
	return texture2D( iChannel0, .5+(vec2(x))/RENDERSIZE.x*SAMPLERADIUS, RENDERSIZE.y);
}

vec4 Rand( in int x )
{
	vec2 uv;
	uv.x = (float(x)+0.0)/128.0;
	uv.y = (floor(uv.y)+0.0)/128.0;
	return texture2D( iChannel0, uv, -10.0 );
}


void mainImage( out vec4 fragColor, in vec2 fragCoord )
{
	vec3 ray;
	ray.xy = 10.*(fragCoord.xy-iResolution.xy*0.5)/iResolution.xy;
	ray.z = 1.0-OVERLOAD;

	float offset = iGlobalTime*.01;	
	
	vec3 col = vec3(0);
	
	vec3 stp = ray/max(abs(ray.x),abs(ray.y));
	
	vec3 pos = 1.0*stp+.5;
	for ( int i=0; i < 128; i++ )
	{
		
		if (i > int(SAMPLES)) { break; }
		
		float z = Noise(ivec2(pos.xy)).x*EXTRUSION;
		z = fract(z-offset*TIMEDILATION+TIMEOFFSET);
		float d = DEPTH*z-pos.z;
		float w = pow(max(0.0,1.0+CHUNKINESS*length(fract(pos.xy)-.5)),2.0);
		vec3 c = max(vec3(0),vec3(1.0-abs(d+WARPFACTORB*.5)/WARPFACTORA,1.0-abs(d)/WARPFACTORA,1.0-abs(d-WARPFACTORB*.5)/WARPFACTORA));
		col += 1.5*(1.0-z)*c*w;
		pos += stp;
	}
	
	fragColor = vec4(ToGamma(col),1.0);
}

void main(void) {
    mainImage(gl_FragColor, gl_FragCoord.xy);
}