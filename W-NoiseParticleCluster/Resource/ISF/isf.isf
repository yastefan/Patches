/*{
	"CREDIT": "by mojovideotech",
	"DESCRIPTION": "",
	"CATEGORIES": [
		"Worley noise"
	],
	"INPUTS": [
	{
		"NAME" : 		"scale",
		"TYPE" : 		"float",
		"DEFAULT" : 	40.0,
		"MIN" : 		1.0,
		"MAX" : 		100.0
	},
	{
		"NAME" : 		"rate",
		"TYPE" : 		"float",
		"DEFAULT" : 	1.0,
		"MIN" : 		0.0,
		"MAX" : 		3.0
	},
	{
		"NAME" : 		"gravity",
		"TYPE" : 		"float",
		"DEFAULT" : 	0.1,
		"MIN" : 		0.01,
		"MAX" : 		0.5
	},
	{
		"NAME" : 		"density",
		"TYPE" : 		"float",
		"DEFAULT" : 	0.15,
		"MIN" : 		0.001,
		"MAX" : 		0.5
	},
	{
		"NAME" : 		"fade",
		"TYPE" : 		"float",
		"DEFAULT" : 	16.0,
		"MIN" : 		2.0,
		"MAX" : 		100.0
	},
	{
		"NAME" : 		"jitter",
		"TYPE" : 		"float",
		"DEFAULT" : 	0.9,
		"MIN" : 		0.25,
		"MAX" : 		0.999
	},
	{
		"NAME" : 		"depth",
		"TYPE" : 		"float",
		"DEFAULT" : 	0.95,
		"MIN" : 		0.1,
		"MAX" : 		2.5
	},
	{
		"NAME" : 		"multiply",
		"TYPE" : 		"float",
		"DEFAULT" : 	0.75,
		"MIN" : 		0.0,
		"MAX" : 		1.0
	},
	{
		"NAME": "cells",
		"TYPE": "bool",
		"DEFAULT": 0.0
	}
	]
}*/


////////////////////////////////////////////////////////////
// W-NoiseParticleCluster  by mojovideotech
//
// based on :
// shadertoy.com/\XlSBDz
//
// Creative Commons Attribution-NonCommercial-ShareAlike 3.0 *
// 
// * cellular2x2x2 ("Worley noise") in 3D in GLSL.
// © 2011 Stefan Gustavson  All rights reserved under MIT license.
////////////////////////////////////////////////////////////


vec4 permute(vec4 x) {
  return mod(((34.0 * x) + 1.0) * x, 289.0);
}

vec2 cellular2x2x2(vec3 P) {
	#define K 0.142857142857 // 1/7
	#define Ko 0.428571428571 // 1/2-K/2
	#define K2 0.020408163265306 // 1/(7*7)
	#define Kz 0.166666666667 // 1/6
	#define Kzo 0.416666666667 // 1/2-1/6*2
	vec3 Pi = mod(floor(P), 289.0);
 	vec3 Pf = fract(P);
	vec4 Pfx = Pf.x + vec4(0.0, -1.0, 0.0, -1.0);
	vec4 Pfy = Pf.y + vec4(0.0, 0.0, -1.0, -1.0);
	vec4 p = permute(Pi.x + vec4(0.0, 1.0, 0.0, 1.0));
	p = permute(p + Pi.y + vec4(0.0, 0.0, 1.0, 1.0));
	vec4 p1 = permute(p + Pi.z); 
	vec4 p2 = permute(p + Pi.z + vec4(1.0)); 
	vec4 ox1 = fract(p1*K) - Ko;
	vec4 oy1 = mod(floor(p1*K), 7.0)*K - Ko;
	vec4 oz1 = floor(p1*K2)*Kz - Kzo;
	vec4 ox2 = fract(p2*K) - Ko;
	vec4 oy2 = mod(floor(p2*K), 7.0)*K - Ko;
	vec4 oz2 = floor(p2*K2)*Kz - Kzo;
	vec4 dx1 = Pfx + jitter*ox1;
	vec4 dy1 = Pfy + jitter*oy1;
	vec4 dz1 = Pf.z + jitter*oz1;
	vec4 dx2 = Pfx + jitter*ox2;
	vec4 dy2 = Pfy + jitter*oy2;
	vec4 dz2 = Pf.z - 1.0 + jitter*oz2;
	vec4 d1 = dx1 * dx1 + dy1 * dy1 + dz1 * dz1; 
	vec4 d2 = dx2 * dx2 + dy2 * dy2 + dz2 * dz2;
#if 0
	d1 = min(d1, d2);
	d1.xy = min(d1.xy, d1.wz);
	d1.x = min(d1.x, d1.y);
	return sqrt(d1.xx);
#else
	vec4 d = min(d1,d2);
	d2 = max(d1,d2); 
	d.xy = (d.x < d.y) ? d.xy : d.yx;
	d.xz = (d.x < d.z) ? d.xz : d.zx;
	d.xw = (d.x < d.w) ? d.xw : d.wx; 
	d.yzw = min(d.yzw, d2.yzw); 
	d.y = min(d.y, d.z); 
	d.y = min(d.y, d.w); 
	d.y = min(d.y, d2.x); 
	return sqrt(d.xy); 
#endif
}

void main() {
	float T = TIME * rate, w, c;
    vec2 uv = (gl_FragCoord.xy - RENDERSIZE.xy*0.5)/RENDERSIZE.y;
   	w = depth - dot(uv, uv)*fade;
    uv *= 101.0-scale;    
    vec2 ip = floor(uv);
    vec2 v = cellular2x2x2(vec3(uv, T));
    if (cells) c = v.y - v.x;
    	else c = v.x;
   	c -= gravity*w;
    c = smoothstep(0.0, max(density/c, multiply), c);
    vec3 col = 1.0-(mix(vec3(0.0), vec3(1.0), c));
    gl_FragColor = vec4(sqrt(max(col, 0.0)), 1.0);
}
