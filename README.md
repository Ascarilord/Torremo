app Torremo {
  wasp: {
    version: "^0.13.0"
  },
  title: "Torremo",
  client: {
    rootComponent: import { Layout } from "@src/Layout.jsx",
  },
  db: {
    prisma: {
      clientPreviewFeatures: ["extendedWhereUnique"]
    }
  },
  auth: {
    userEntity: User,
    methods: {
      usernameAndPassword: {}
    },
    onAuthFailedRedirectTo: "/login",
    onAuthSucceededRedirectTo: "/"
  },
}

route LoginRoute { path: "/login", to: LoginPage }
page LoginPage {
  component: import Login from "@src/pages/auth/Login.jsx"
}
route SignupRoute { path: "/signup", to: SignupPage }
page SignupPage {
  component: import Signup from "@src/pages/auth/Signup.jsx"
}

entity User {=psl
    id               Int    @id @default(autoincrement())
    resume           String
    subscriptionType String
    paymentMethod    String
    Job              Job[]
psl=}

entity Job {=psl
    id              Int    @id @default(autoincrement())
    position        String
    competencyLevel Float  // Updated to store competency level as a float
    user            User   @relation(fields: [userId], references: [id])
    userId          Int
psl=}

entity Employer {=psl
    id               Int    @id @default(autoincrement())
    resume           String
    subscriptionType String
    paymentMethod    String
psl=}

action uploadResume {
  fn: import { uploadResume } from "@src/actions.js",
  entities: [User]
}

action applyForJob {
  fn: import { applyForJob } from "@src/actions.js",
  entities: [User, Job]
}

action subscribe {
  fn: import { subscribe } from "@src/actions.js",
  entities: [User]
}

action enhanceResume {
  fn: import { enhanceResume } from "@src/actions.js",
  entities: [User]
}

action employerUploadResume {
  fn: import { employerUploadResume } from "@src/actions.js",
  entities: [Employer]
}

action employerSubscribe {
  fn: import { employerSubscribe } from "@src/actions.js",
  entities: [Employer]
}

query getResume {
  fn: import { getResume } from "@src/queries.js",
  entities: [User]
}

query getSubscription {
  fn: import { getSubscription } from "@src/queries.js",
  entities: [User]
}

query getEmployerResume {
  fn: import { getEmployerResume } from "@src/queries.js",
  entities: [Employer]
}

query getEmployerSubscription {
  fn: import { getEmployerSubscription } from "@src/queries.js",
  entities: [Employer]
}

route HomePageRoute { path: "/", to: HomePage }
page HomePage {
  component: import HomePage from "@src/pages/Home.jsx",
  authRequired: false
}

route JobSeekerRoute { path: "/jobseeker", to: JobSeekerPage }
page JobSeekerPage {
  component: import JobSeekerPage from "@src/pages/JobSeeker.jsx",
  authRequired: true
}

route EmployerRoute { path: "/employer", to: EmployerPage }
page EmployerPage {
  component: import EmployerPage from "@src/pages/Employer.jsx",
  authRequired: true
}

// Additional actions and queries for resume handling and competency calculation
action calculateCompetency {
  fn: import { calculateCompetency } from "@src/actions.js",
  entities: [Job]
}

action suggestRolesForApplicant {
  fn: import { suggestRolesForApplicant } from "@src/actions.js",
  entities: [Job]
}

action verifyApplicantEligibility {
  fn: import { verifyApplicantEligibility } from "@src/actions.js",
  entities: [Job]
}

action processSubscriptionPayment {
  fn: import { processSubscriptionPayment } from "@src/actions.js",
  entities: [User]
}

action processEmployerSubscriptionPayment {
  fn: import { processEmployerSubscriptionPayment } from "@src/actions.js",
  entities: [Employer]
}

// Additional queries for retrieving and displaying resume and subscription information
query getEmployerEligibleApplicants {
  fn: import { getEmployerEligibleApplicants } from "@src/queries.js",
  entities: [Job]
}

query getUserSuitableRoles {
  fn: import { getUserSuitableRoles } from "@src/queries.js",
  entities: [Job]
}
