/*{
	"CREDIT": "by echophons",
	"DESCRIPTION": "",
	"CATEGORIES": [ "generator"
	],
	"INPUTS": [
	
		{
			"NAME": "k",
			"TYPE": "float",
			"DEFAULT": 0.3,
			"MIN": 0.001,
			"MAX": 0.999
		},
		{
			"NAME": "h",
			"TYPE": "float",
			"DEFAULT": 0.5,
			"MIN": 0.0,
			"MAX": 1.0
		},
		{
			"NAME": "j",
			"TYPE": "float",
			"DEFAULT": 0.5,
			"MIN": 0.0,
			"MAX": 1.0
		},
			{
			"NAME": "c",
			"TYPE": "float",
			"DEFAULT": 0.632,
		    "MIN": 0.001,
			"MAX":0.999
		},
			{
			"NAME": "s",
			"TYPE": "float",
			"DEFAULT": 0.131,
			"MIN": 0.001,
			"MAX":0.999
		},
			{
			"NAME": "e",
			"TYPE": "float",
			"DEFAULT": 0.3,
		    "MIN": 0.001,
			"MAX":0.999
		},
			{
			"NAME": "u",
			"TYPE": "float",
			"DEFAULT": 0.17,
			"MIN": 0.001,
			"MAX":0.999
		},
		{
			"NAME": "m",
			"TYPE": "float",
			"DEFAULT": 0.3,
			"MIN": 0.001,
			"MAX": 0.999
		}
	]
}*/

// edit of http://glslsandbox.com/e#18752.0
// additional iputs added by Doctor Mojo
uniform vec2 mouse;

vec3   iResolution = vec3(RENDERSIZE, 1.0);
float  iGlobalTime = TIME;

float gTime = iGlobalTime*0.5;

void main( void )
{
	float f = 1.0;
	float g = 1.0;
	vec2 res = iResolution.xy;
	vec2 mou = mouse.xy;
	
	//if (mouse.x < 0.5)
	//{
	mou.x = sin(gTime * e)*sin(gTime * u) * 1. + sin(gTime * m);
	mou.y = (1.0-cos(gTime * c))*sin(gTime * s)*1.0+cos(gTime * k);
	mou = (mou+1.0) * res;
	//}
	vec2 z = ((-res+2.0 * gl_FragCoord.xy) / res.y);
	vec2 p = ((-res+2.0+mou) / res.y) * j;
	for( int i = 0; i < 24; i++) 
	{
		float d = dot(z,z);
		z = (vec2( z.x, -z.y ) / d) + p * h; 
		z.x =  1.0-abs(z.x);
		f = max( f-d, (dot(z-p,z-p) ));
		g = min( g*d, sin(dot(z+p,z+p))+1.0);
	}
	f = abs(-log(f) / 3.5);
	g = abs(-log(g) / 8.0);
	gl_FragColor = vec4(min(vec3(g, g*f, f), 1.0),1.0);
}