# PINN_turbulent_jet
**PINN for data assimilation for axisymmetric turbulent jet using temperature fields.** 

The data assimilation technique for turbulent flows with PINN without a specific turbulence model is proposed, which allows determination of the turbulent viscosity, velocity and pressure distributions from the experimentally measured or synthetic temperature fields. There is an example with measurements are performed for a round jet of hot air using nonintrusive Background Oriented Schlieren (BOS) technique. Experimental data and fields from similar simulations using Spalart–Allmaras and $k-ε$ turbulence models are availiable for downloading by links in folder [experimental_BOS_temperature_fields](/experimental_BOS_temperature_fields/Links).

A notebook with example for one experimental series is availiable [here](/PINN_notebook/PINN_turbulent_jet.ipynb).

Besides, simple autoencoder (simpleAE) has been trained using numerical fields from simulation of turbulent jet with the modified Spalart–Allmaras model. Then physics loss (that is equaitions residuals) were included in loss-function, but the predictions were spoilt. So it was shown that numerical derivatives and autioencoder didn't work for data assimilation in this case. A notebook is availiable [here](simpleAE/simpleAE_numerical_derivatives.ipynb).

As the raw experimental data are the displacement fields (not temperature fields) the [Denoising autoencoder](https://github.com/GuFeng-95/Denoising-Autoencoder) is used for filtering displacement fields obtained in BOS experiment. Filtered fields can be then used for processing to obtain more smooth temperature fields. A notebook is availiable [here](DAE/DAE_filtering_displacement_fields_training.ipynb).

*Illustration of full-connected neural network*:

![alt text](https://github.com/YulieRu/PINN_turbulent_jet/blob/7f5ceb3dca788ba5b68e9714223ac9ececd9f29b/scheme.svg)

Equations in the approximation of a weakly compressible medium are used: 

**Continuity equation:**

$$\frac{1}{r}\frac{\partial (r\rho v_{r})}{\partial r} + \frac{\partial \rho v_{z}}{\partial z}=0$$

**R-component equation:**


$$\rho\left[ v_r \frac{\partial v_r}{\partial r} + v_z \frac{\partial v_r}{\partial z} \right] = - \frac{\partial p}{\partial r} +\frac{\tilde{\mu_0}}{Re_0} \frac{\partial}{\partial r} \left[\mu \left( -\frac{2}{3} \nabla v + 2\frac{\partial v_r}{\partial r} \right)\right] + \frac{\tilde{\mu_0}}{Re_0}\frac{\partial}{\partial z} \left[\mu\left( \frac{\partial v_r}{\partial z} +\frac{\partial v_z}{\partial r}\right)\right] + \frac{2\tilde{\mu_0}}{Re_0}\frac{\mu}{r}\left[\frac{\partial v_r}{\partial r} - \frac{v_r}{r} \right]$$

**Z-component equation:**

$$\rho\left[ v_r \frac{\partial v_z}{\partial r} + v_z \frac{\partial v_z}{\partial z} \right] = - \frac{\partial p}{\partial z} +\frac{\tilde{\mu_0}}{Re_0} \frac{\partial}{\partial z} \left[\mu \left( -\frac{2}{3} \nabla v + 2\frac{\partial v_z}{\partial z} \right)\right] + \frac{\tilde{\mu_0}}{Re_0}\frac{\partial}{\partial r} \left[\mu\left( \frac{\partial v_z}{\partial r} +\frac{\partial v_r}{\partial z}\right)\right] + \frac{\tilde{\mu_0}}{Re_0}\frac{\mu}{r}\left[\frac{\partial v_r}{\partial z} + \frac{\partial v_z}{\partial r} \right]$$

**Energy equation:**
$$\rho\left[ v_r \frac{\partial T}{\partial r} + v_z \frac{\partial T}{\partial z} \right] = \frac{1}{Pe_0} \frac{1}{r}\frac{\partial }{\partial r} \left (\lambda r \frac{\partial T}{\partial r} \right) + \frac{1}{Pe_0} \frac{\partial }{\partial z} \left (\lambda \frac{\partial T}{\partial z} \right)$$

where:
$$\nabla v = \frac{1}{r}\frac{\partial}{\partial r}\left(r v_r \right) + \frac{\partial v_z}{\partial z}$$ 
$$\rho = \frac{\rho_{dim}}{\rho_0}$$
$$\rho_{dim} = \frac{p_0 M}{R(T+T_0+273.15)}$$
$$\rho_0 = \frac{p_0 M}{R(T_0+273.15)}$$
$$\lambda = 1 + \frac{C_p}{Pr_t} \frac{\mu_{mol}}{\lambda_{mol}} \left( \mu - 1 \right)$$
$$Re = \frac{\rho_0 v_0 l}{\mu_{mol}}, Pe = \frac{\rho_0 C_p v_0 l}{\lambda_{mol}}$$
Normalization:
$$r, z : l$$
$$v_r, v_z : v_0$$
$$p : \rho_0 v_0^{2}$$
$$T : T_{norm}$$
$$\mu : \mu_{mol}$$
$$\tilde{\mu_0} = const \ (>= max \ \mu \ from \ all \ experiments)$$
Also the replacement $v_r=ar$ were made to avoid big residuals due to the memebers proportional $\frac{1}{r}$.
