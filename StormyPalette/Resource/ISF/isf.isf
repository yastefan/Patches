/*
{
  "CATEGORIES" : [
    "XXX"
  ],
  "DESCRIPTION" : "",
  "ISFVSN" : "2",
  "INPUTS" : [
  {
      "NAME" : "Scaling",
      "TYPE" : "float",
      "MAX" : 3.0,
      "DEFAULT" : 0.7,
      "MIN" : 0.05
    },
  {
      "NAME" : "Calm",
      "TYPE" : "float",
      "MAX" : 4.0,
      "DEFAULT" : 1.0,
      "MIN" : 0.001
    },
    {
      "NAME" : "Contrast",
      "TYPE" : "float",
       "MAX" : 1.4,
      "DEFAULT" : 1.1,
      "MIN" : 0.7    },
    {
			"NAME": "Color1",
			"TYPE": "color",
			"DEFAULT": [
				0.9,
				0.7,
				0.2,
				1.0
			]
		},
		{
			"NAME": "Color2",
			"TYPE": "color",
			"DEFAULT": [
				0.3,
				0.7,
				1.0,
				1.0
			]
		}		
  ],
  "CREDIT" : ""
}
*/
//By Silvia Fabiani
// Based on @patriciogv - 2015
// http://patriciogonzalezvivo.com
// Based on Morgan McGuire @morgan3d
// https://www.shadertoy.com/view/4dS3WDATE

#ifdef GL_ES
precision mediump float;
#endif

float random (in vec2 _st) {
    return fract(sin(dot(_st.xy,
                         vec2(12.9898,78.233)))*
        43758.5453123);
}


float noise (in vec2 _st) {
    vec2 i = floor(_st);
    vec2 f = fract(_st);

    // Four corners in 2D of a tile
    float a = random(i);
    float b = random(i + vec2(1.0, 0.0));
    float c = random(i + vec2(0.0, 1.0));
    float d = random(i + vec2(1.0,1.000));

    vec2 u = f * f * (3.0 - 2.0 * f);

    return mix(a, b, u.x) +
            (c - a)* u.y * (1.0 - u.x) +
            (d - b) * u.x * u.y;
}

#define NUM_OCTAVES 5
float TT = clamp(TIME,0.0,12.0);
float fbm ( in vec2 _st) {
    float v = -0.5;
    float a = Contrast;
    vec2 shift = vec2(100.0);
    // Rotate to reduce axial bias
    mat2 rot = mat2(cos(Calm), sin(1.02), -sin(0.5), cos(- 0.001));
    for (int i = 0; i < NUM_OCTAVES; ++i) {
        v += a * noise(_st);
        _st = rot * _st * 2.0 + shift;
        a *= 0.5;
    }
    return v;
}

void main() {
    vec2 st = gl_FragCoord.xy/RENDERSIZE.xy*3.;
    st += st * Scaling ;
    vec3 color = vec3(0.0);

    vec2 q = vec2(0.);
    q.x = fbm( st + 0.00*TIME);
    q.y = fbm( st + vec2(1.0));

    vec2 r = vec2(0.);
    r.x = fbm( st + 1.0*q + vec2(1.7,9.2)+ 0.15*TIME );
    r.y = fbm( st + 1.0*q + vec2(8.3,2.8)+ 0.126 * TIME);

    float f = fbm(st+r);
    color = mix(vec3(0.2,0.2,0.6),
                vec3(0.1,0.1,0.5),
                clamp((f*f)*-0.096,0.0,0.0));

    color = mix(color,
                vec3(Color1 + cos (TIME/TT/2.0)),
                clamp(length(q),0.0,1.0));

    color = mix(color,
                vec3(Color2 + sin (TIME/TT/2.0)),
                clamp(length(r.x),0.0,1.0));

    gl_FragColor = vec4((f*f*f+.6 * f*f+.5*f)*color,1.);
}

