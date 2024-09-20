# i3

super
```bash
bindsym $sup+t exec i3-sensible-terminal
bindsym $sup+o exec obsidian
bindsym $sup+c exec google-chrome
bindsym $sup+shift+c personal chrome 
bindsym $sup+shift+a personal chrome chat gpt 
```

mod
```bash
bindsym $mod+q kill
bindsym $mod+p exec --no-startup-id flameshot gui
bindsym $mod+Shift+percent split h
bindsym $mod+Shift+apostrophe split v
bindsym $mod+f fullscreen toggle
bindsym $mod+s layout stacking
bindsym $mod+w layout tabbed
bindsym $mod+e layout toggle split
bindsym $mod+shift+space floating toggle
```

# dwm

super
```c
{ SUPERKEY,		XK_d,      spawn,          {.v = dmenucmd } },
{ SUPERKEY,		XK_t,	   spawn,          {.v = termcmd } },
{ SUPERKEY,		XK_c,	   spawn,          {.v = vivaldicmd } },
{ SUPERKEY,		XK_o,	   spawn,          {.v = obsidiancmd } },
```

mod
```c
{ MODKEY,             XK_p,      spawn,		   {.v = flameshotcmd } },
{ MODKEY,             XK_b,      togglebar,      {0} },
{ MODKEY,             XK_j,      focusstack,     {.i = +1 } },
{ MODKEY,             XK_k,      focusstack,     {.i = -1 } },
{ MODKEY,             XK_i,      incnmaster,     {.i = -1 } },
{ MODKEY|ShiftMask,   XK_i,      incnmaster,     {.i = +1 } },
{ MODKEY,             XK_h,      setmfact,       {.f = -0.05} },
{ MODKEY,             XK_l,      setmfact,       {.f = +0.05} },
{ MODKEY,             XK_Return, zoom,           {0} },
{ MODKEY,             XK_Tab,    view,           {0} },
{ MODKEY,		      XK_q,      killclient,     {0} },
{ MODKEY,             XK_e,      setlayout,      {.v = &layouts[0]} },
{ MODKEY,             XK_s,      setlayout,      {.v = &layouts[1]} },
{ MODKEY,             XK_w,      setlayout,      {.v = &layouts[2]} },
{ MODKEY,             XK_space,  setlayout,      {0} },
{ MODKEY|ShiftMask,   XK_space,  togglefloating, {0} },
{ MODKEY,             XK_0,      view,           {.ui = ~0 } },
{ MODKEY|ShiftMask,   XK_0,      tag,            {.ui = ~0 } },
{ MODKEY,             XK_comma,  focusmon,       {.i = -1 } },
{ MODKEY,             XK_period, focusmon,       {.i = +1 } },
{ MODKEY|ShiftMask,   XK_comma,  tagmon,         {.i = -1 } },
{ MODKEY|ShiftMask,   XK_period, tagmon,         {.i = +1 } },
{ MODKEY|ShiftMask,   XK_q,      quit,           {0} },
```

# bspwm
>remember to install sxhkd for keyboard and mouse events

super
```bash
```

mod
```bash
```