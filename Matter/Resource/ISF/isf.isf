/*{
	"CREDIT": "by zebox02",
	"DESCRIPTION": "",
	"CATEGORIES": [
		""
	],
	"INPUTS": [
		{
			"NAME": "scale",
			"TYPE": "float",
			"DEFAULT": 100.0,
			"MIN": 0.0,
			"MAX": 1000.0
		},
		{
			"NAME": "amp",
			"TYPE": "float",
			"DEFAULT": 0.4,
			"MIN": 0.0,
			"MAX": 1.0
		},
		{
			"NAME": "r1",
			"TYPE": "float",
			"DEFAULT": 1.0,
			"MIN": 0.0,
			"MAX": 1.0
		},
		{
			"NAME": "r2",
			"TYPE": "float",
			"DEFAULT": 0.8,
			"MIN": 0.0,
			"MAX": 1.0
		},
		{
			"NAME": "r3",
			"TYPE": "float",
			"DEFAULT": 0.6,
			"MIN": 0.0,
			"MAX": 1.0
		},
		{
			"NAME": "r4",
			"TYPE": "float",
			"DEFAULT": 0.7,
			"MIN": 0.0,
			"MAX": 1.0
		},
		{
			"NAME": "g1",
			"TYPE": "float",
			"DEFAULT": 0.91,
			"MIN": 0.0,
			"MAX": 1.0
		},
		{
			"NAME": "g2",
			"TYPE": "float",
			"DEFAULT": 0.88,
			"MIN": 0.0,
			"MAX": 1.0
		},
		{
			"NAME": "g3",
			"TYPE": "float",
			"DEFAULT": 0.35,
			"MIN": 0.0,
			"MAX": 1.0
		},
		{
			"NAME": "g4",
			"TYPE": "float",
			"DEFAULT": 0.6,
			"MIN": 0.0,
			"MAX": 1.0
		},
		{
			"NAME": "b1",
			"TYPE": "float",
			"DEFAULT": 0.98,
			"MIN": 0.0,
			"MAX": 1.0
		},
		{
			"NAME": "b2",
			"TYPE": "float",
			"DEFAULT": 0.7,
			"MIN": 0.0,
			"MAX": 1.0
		},
		{
			"NAME": "b3",
			"TYPE": "float",
			"DEFAULT": 0.4,
			"MIN": 0.0,
			"MAX": 1.0
		},
		{
			"NAME": "b4",
			"TYPE": "float",
			"DEFAULT": 0.85,
			"MIN": 0.0,
			"MAX": 1.0
		},
		{
			"NAME": "c_5",
			"TYPE": "float",
			"DEFAULT": 0.2,
			"MIN": 0.0,
			"MAX": 1.0
		},
		{
			"NAME": "c_6",
			"TYPE": "float",
			"DEFAULT": 0.9,
			"MIN": 0.0,
			"MAX": 1.0
		}
	]
}*/

float rand(vec2 n) { 
    return fract(cos(dot(n, vec2(12.9898, 4.1414))) * 43758.5453);
}

float noise(vec2 n) {
    const vec2 d = vec2(0.0, 1.0);
    vec2 b = floor(n), f = smoothstep(vec2(0.0), vec2(1.0), fract(n));
    return mix(mix(rand(b), rand(b + d.yx), f.x), mix(rand(b + d.xy), rand(b + d.yy), f.x), f.y);
}

float fbm(vec2 n) {
    float total = 0.0, amplitude = .9;
    for (int i = 0; i < 7; i++) {
        total += sin(noise(n) * amplitude);
        n += n;
        amplitude *= .9;
    }
    return total;
}

void main() {
	float t = TIME;
    vec3 c1 = vec3(r1, g1, b1);
    vec3 c2 = vec3(r2, g2, b2);
    vec3 c3 = vec3(r3, g3, b3);
    vec3 c4 = vec3(r4, g4, b4);
    vec3 c5 = vec3(c_5);
    vec3 c6 = vec3(c_6);
    vec2 p = gl_FragCoord.xy / 100.0;
    float q = fbm(p - t * 0.1);
    vec2 r = vec2(fbm(p + q + t - p.x - p.y), fbm(p + q - t * .9));
    vec3 c = mix(c1, c2, fbm(p + r * t)) + mix(c3, c4, r.x) - mix(c5, c6, r.y);
    gl_FragColor = vec4(c, 1.0);
}