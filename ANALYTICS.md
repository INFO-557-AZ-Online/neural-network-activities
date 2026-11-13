# Privacy and activity progress

This INFO 557 GitHub Pages deployment does **not** load Google Analytics or send
activity answers, reflections, click events, completion or timing to an analytics
service. The generator includes no analytics tag; `docs/assets/analytics.js` is an
inert compatibility file. Debug URL parameters do not turn tracking on.

Activity calculations and feedback run in the browser. Completion badges reset
when a page reloads; they are not an instructor dashboard, attendance record,
submission or D2L grade. No login or student identifier is requested on Pages.
GitHub Pages receives ordinary hosting requests. The math renderer and its CSS
load from jsDelivr; this is a network dependency, not a course analytics stream.

The local FastAPI backend is a separate legacy development option and contains a
CSV submission endpoint. GitHub Pages does not run Python, expose that endpoint
or include the submission function in its generated JavaScript. For local static
practice, serve `docs/` as described in the README. Do not treat the development
backend as an approved student-data collection service.

The inherited analytics/report implementation is retained in Git history. The
SQL report in `analysis/activities_report.sql` is historical and does not describe
collection by this deployment. Any future analytics change is a separate decision;
this publication does not configure a GA property or collect research records.
