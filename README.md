# Sin-Generator-with-Exponential-Rise

#include <stdio.h>
#include <math.h>

int main(void)
{
    FILE *out;
    float samp = 0.0;

    out = fopen("exponential_sweep.bin", "wb");

    float phi = 0.0;
    float f_start = 1.0;
    float f_end = 48000.0;
    float duration = 60.0;
    float sampleRate = 48000.0;
    float numSamples = sampleRate * duration;
    
    // Exponential growth rate
    float k = log(f_end / f_start) / duration;

    for (int i = 0; i < numSamples; i++)
    {
        float t = (float)i / sampleRate;
        
        // Exponentially increasing frequency
        float freq = f_start * exp(k * t);  
        
        // Phase increment based on current frequency
        phi = phi + (2.0 * M_PI * freq / sampleRate);

        if (phi > (2 * M_PI))
        {
            phi = phi - (2 * M_PI);
        }
        
        samp = sin(phi);
        fwrite(&samp, sizeof(float), 1, out);
    }

    printf("Done!\n");

    fclose(out);
    return 0;
}
