/*
{
  "CATEGORIES" : [
    "Generator"
  ],
  "DESCRIPTION" : "Twin Rays",
  "ISFVSN" : "2",
  "INPUTS" : [
    {
      "NAME" : "rate",
      "TYPE" : "float",
      "MAX" : 0.059999999999999998,
      "DEFAULT" : -0.016063844785094261,
      "LABEL" : "SPEED",
      "MIN" : -0.059999999999999998
    },
    {
      "NAME" : "colorIN",
      "TYPE" : "color",
      "DEFAULT" : [
        1,
        0.20000000298023224,
        0.10000000149011612,
        1
      ],
      "LABEL" : "Color"
    },
    {
      "NAME" : "Count",
      "TYPE" : "float",
      "MAX" : 25,
      "DEFAULT" : 18.131168365478516,
      "LABEL" : "Ray Count",
      "MIN" : 3
    },
    {
      "NAME" : "posY",
      "TYPE" : "float",
      "MAX" : 0.5,
      "DEFAULT" : -0.33067807555198669,
      "LABEL" : "Position Y",
      "MIN" : -0.5
    },
    {
      "NAME" : "posX",
      "TYPE" : "float",
      "MAX" : 0.5,
      "DEFAULT" : 0.1662137508392334,
      "LABEL" : "Position X",
      "MIN" : 0
    },
    {
      "NAME" : "width",
      "TYPE" : "float",
      "MAX" : 0.45,
      "DEFAULT" : 0.2891484797000885,
      "LABEL" : "Ray Width",
      "MIN" : 0.01
    },
		{
			"NAME" : "soft",
			"TYPE" : "float",
			"MAX" : 0.99,
			"DEFAULT" : 0.2891484797000885,
			"LABEL" : "Ray Softness",
			"MIN" : 0.01
		}
  ],
  
  "CREDIT" : "howie.tv"
}
*/
// its a  howie.tv thing

// _read the time buffer

#define M_PI 3.1415926535897932384626433832795


// rgb2hsv
vec3 rgb2hsv(vec3 c)
{
    vec4 K = vec4(0.0, -1.0 / 3.0, 2.0 / 3.0, -1.0);
    vec4 p = mix(vec4(c.bg, K.wz), vec4(c.gb, K.xy), step(c.b, c.g));
    vec4 q = mix(vec4(p.xyw, c.r), vec4(c.r, p.yzx), step(p.x, c.r));
    float d = q.x - min(q.w, q.y);
    float e = 1.0e-10;
    return vec3(abs(q.z + (q.w - q.y) / (6.0 * d + e)), d / (q.x + e), q.x);
}

// hsv2rgb
vec3 hsv2rgb(vec3 c)
{
    vec4 K = vec4(1.0, 2.0 / 3.0, 1.0 / 3.0, 3.0);
    vec3 p = abs(fract(c.xxx + K.xyz) * 6.0 - K.www);
    return c.z * mix(K.xxx, clamp(p - K.xxx, 0.0, 1.0), c.y);
}

float rays(vec2 uv, float c, float w, float s)
	{
		uv.x = fract(uv.x*c)-0.5;
	 	return smoothstep(-w , -w*s, uv.x) * smoothstep(w, w*s, uv.x);
	}

void main() {

				float ratio = RENDERSIZE.y/RENDERSIZE.x;
				vec2 uv = vec2( isf_FragNormCoord.x -0.5, (isf_FragNormCoord.y -0.5) * ratio);
				
				vec3 color;
				vec3 col;
				vec2 UV;
				float m;
				float angle;
				float radius;
				float count = floor(Count);
				uv.y -= posY;

				vec3 hvs = rgb2hsv(colorIN.rgb);
				vec3 colorA = hsv2rgb(vec3(hvs.x +0.1, hvs.y, hvs.z));
				vec3 colorB = hsv2rgb(vec3(hvs.x -0.1, hvs.y, hvs.z));

				for(int i = 0;  i< 2; i++)
				{
					UV = uv;
					UV.x -= posX*(1.0-(float(i)*2.0));
					UV.x *= 1.0-(float(i)*2.0);
					angle = atan(UV.y, UV.x);
					angle = angle/((M_PI*4.0)*0.5) + TIME*rate	;
					radius = length(UV);
					UV = vec2(angle, radius*1.5);
					col = mix(colorA,colorB, abs(UV.y)/ratio);


					m = rays(UV,count,width,soft);
					m = pow(m,3.0);
					color += col*m;
				}


	gl_FragColor = vec4(color,1.0);
}
