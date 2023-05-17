/*
 {
  "DESCRIPTION": "black hole sun",
  "CREDIT": "Andrea Bovo <https://linktr.ee/spleenooname>", 
  "CATEGORIES": [
    "Distortion Effect",
    "Warp",
    "Black White",
    "black",
    "white"
  ],
  "INPUTS": [
    {
      "NAME": "brightness",
      "TYPE": "float",
      "DEFAULT": 4,
      "MIN": 0,
      "MAX": 5
    },
    {
      "NAME": "ray_brightness",
      "TYPE": "float",
      "DEFAULT": 2.5,
      "MIN": 0,
      "MAX": 10
    },
 
    {
      "NAME": "spot_brightness",
      "TYPE": "float",
      "DEFAULT": 15,
      "MIN": -15,
      "MAX": 15
    },
    {
      "NAME": "ray_density",
      "TYPE": "float",
      "DEFAULT": 12,
      "MIN": 0,
      "MAX": 100
    },
    {
      "NAME": "curvature",
      "TYPE": "float",
      "DEFAULT": 300,
      "MIN": 1,
      "MAX": 1080
    },
    {
      "NAME": "angle",
      "TYPE": "float",
      "DEFAULT": 0.5,
      "MIN": 0,
      "MAX": 2
    },
    {
      "NAME": "freq",
      "TYPE": "float",
      "DEFAULT": 5,
      "MIN": 1,
      "MAX": 10
    },
    {
      "NAME": "warp",
      "TYPE": "bool",
      "DEFAULT": 1
    }
  ]
}*/

// based on : Flaring by nimitz - https://www.shadertoy.com/view/lsSGzy

#define time TIME
#define R RENDERSIZE

// Gradient noise by iq - https://www.shadertoy.com/view/XdXGW8
vec2 hash( vec2 x )  {
    const vec2 k = vec2( 0.3183099, 0.3678794 );
    x = x*k + k.yx;
    return -1.0 + 2.0*fract( 64.0 * k*fract( x.x*x.y*(x.x+x.y)) );
}

float noise( in vec2 p ){
    vec2 i = floor( p );
    vec2 f = fract( p );
	vec2 u = f*f*(3.0-2.0*f);
    return mix( mix( dot( hash( i + vec2(0.0,0.0) ), f - vec2(0.0,0.0) ), 
                     dot( hash( i + vec2(1.0,0.0) ), f - vec2(1.0,0.0) ), u.x),
                mix( dot( hash( i + vec2(0.0,1.0) ), f - vec2(0.0,1.0) ), 
                     dot( hash( i + vec2(1.0,1.0) ), f - vec2(1.0,1.0) ), u.x), u.y);
}


mat2 rot2d( float angle ) {
	float s = sin(angle);
	float c = cos(angle);
    return mat2(c, -s, s, c);
}

#define OCTAVES 6
float fbm( in vec2 p, in float freq ){	
	float z = 1.;
    float rz = 0.;
	p *= 0.25;
	mat2 mrot = rot2d( angle );
	for (int i= 1;i < OCTAVES;i++ ){
		rz += (sin(noise(p) * freq) * 0.5 + 0.5 ) / (z + time * 0.001);
		z *= 1.75; 
		p *= 2.;
		p *= mrot;
	}
	return rz;
}

void main(){
    //
	float t = time * 0.025;
	// thx Fabrice
	vec2 uv =  ( 2. * gl_FragCoord.xy - R ) / min(R.x,R.y);
	
	uv *= curvature * 5e-2;
	
	float r = sqrt( dot(uv, uv) );
	float x = dot( normalize(uv), vec2(.5, 0.) ) + t;	
	float y = dot (normalize(uv), vec2(.0, .5) ) + t;
	
	if (warp) {
	  float d = ray_density * .5;
	  x = fbm( vec2( y * d, r + x * d), freq );
	  y = fbm( vec2( r + y * d, x * d ), freq );
	}

    float val = fbm( vec2(r + y * ray_density, r + x * ray_density - y), freq);
     
	val = smoothstep( 0., ray_brightness, val);
	
	vec3 col = clamp( 1.- vec3(val), 0., 1.);
	
	col = mix(col, vec3(1.0), spot_brightness - 10.* r/curvature * 200./brightness);
	
	gl_FragColor = sqrt(vec4(col, 1.0));
}