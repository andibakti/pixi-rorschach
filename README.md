# pixijs-rorschach

A rorschach generator made with PixiJS as 2D GLSL shader inside a Vue app based on simplex noise. 

## Papers

I wanted to simulate the effects of ink on paper as close as possible within the limits of PixiJS. For those looking to do something similar use a 3D shader.

* Luft, Thomas, and Oliver Deussen. "Interactive watercolor animations." Computer Graphics and Applications (PG'05). 2005.
  -  The implementation is largely based on this. It was the computationaly cheapest approach to realistic watercolor rendering.
* Ebert, David S., et al. _Texturing & modeling: a procedural approach_. Morgan Kaufmann, 2003.
  - I recommend going through chapter 2 to really understand how noise is made and how to transform it into interesting things.
* Rosin, Paul, and John Collomosse, eds. "Image and video-based artistic stylisation." Vol. 42. Springer Science & Business Media, 2012.
  - Chapter 6 specifically has a list of other research related to watercolor rendering approaches. This section also has references to physically accurate approaches
* Chu, Nelson S-H., and Chiew-Lan Tai. "Moxi: real-time ink dispersion in absorbent paper." ACM Transactions on Graphics (TOG) 24.3 (2005): 504-511.
  - The end all be all for ink simulation. Look at the [unity implementation of this paper.](https://github.com/komietty/unity-moxi-ink) It is GPU intensive however which inspired me to make this implementation in the first place.


## Next steps

I'm still disatisfied with how the ink looks. Actual inkblot patterns for Rorschach tests have the ink dry nonuniformly as the liquid percolates. This has the effect of adding more variation to what people can interpret from the patterns. However, having already spent many hours tweeking it to look the way it does now (though I admit a lot of that time was spent starring into the hypnotising patterns), I don't think I can revisit the look anytime soon.

I'd like the implement a commenting system to allow people to share what they "see" in the patterns.

I also want to revise how the initial noise is generated and use random ink depositing based approach like for real inkblots. This would have the added benefit of adding more interesting animations vs. currently where I'm modifying the z axis on the 3D perlin noise.


### Project setup
```
yarn install
```

**Compiles and hot-reloads for development**
```
yarn serve
```
**Compiles and minifies for production**
```
yarn build
```
